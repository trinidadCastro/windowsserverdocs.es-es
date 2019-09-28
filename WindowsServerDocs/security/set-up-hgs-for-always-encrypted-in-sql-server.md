---
title: Configuración del servicio de protección de host para Always Encrypted en SQL Server
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 5d1396f609a425adcd87a41d3469f3aa55774851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402252"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configuración del servicio de protección de host para Always Encrypted con Secure enclaves en SQL Server 

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, SQL Server 2019 Preview
 
[Always Encrypted con Secure enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) en SQL Server 2019 Preview es una característica diseñada para habilitar cálculos confidenciales en datos confidenciales almacenados en una base de datos de. El servicio de protección de host (HGS) juega un rol importante en el mantenimiento de los datos seguros cuando un enclave seguro, configurado para Always Encrypted, es un enclave de memoria de seguridad basada en virtualización (VBS). La seguridad de un enclave de memoria VBS depende de la seguridad del hipervisor de Windows y, en general, la seguridad de la máquina que hospeda SQL Server. 

Por lo tanto, antes de que una aplicación cliente de base de datos permita los enclave de memoria VBS usados para Always Encrypted para realizar cálculos en datos confidenciales, la aplicación debe atestiguar con un HGS de confianza. La atestación demuestra que el equipo que hospeda SQL Server, que contiene enclave, está en el estado correcto y puede ser de confianza. En el resto de este tema, haremos referencia a la máquina que hospeda SQL Server simplemente como el equipo host.

Hay dos maneras mutuamente excluyentes para que la aplicación certifique: 

- La atestación de clave de host autoriza a un host mediante la prueba de que posee una clave privada conocida y de confianza. Se recomienda la atestación de clave de host y un equipo host físico o una máquina virtual de generación 2 que ejecute SQL Server para entornos de preproducción.
- La atestación de TPM valida las medidas de hardware para asegurarse de que un host ejecuta solo los archivos binarios y las directivas de seguridad correctos. Se recomienda la atestación de TPM y un equipo host físico (no una máquina virtual) que ejecute SQL Server para entornos de producción.

Para más información sobre el servicio de protección de host y lo que puede medir, consulte [información general sobre el tejido protegido y las máquinas virtuales blindadas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Tenga en cuenta que, aunque los documentos hablan sobre máquinas virtuales blindadas, se aplican las mismas protecciones, arquitectura y procedimientos recomendados a SQL Server Always Encrypted mediante VBS enclaves. 

Este artículo le ayudará a configurar HGS en una configuración recomendada. 

## <a name="prerequisites"></a>Requisitos previos 

En esta sección se tratan los requisitos previos para HGS y equipos host. 

### <a name="hgs-servers"></a>Servidores HGS

- 1-3 servidores para ejecutar HGS. 

  > [!NOTE]
  > Solo se necesita 1 servidor HGS para un entorno de prueba o de preproducción.

  Estos servidores deben estar protegidos cuidadosamente, ya que controlan qué máquinas pueden ejecutar las instancias de SQL Server mediante Always Encrypted con enclaves seguro. 
  Se recomienda que los distintos administradores administren el clúster de HGS y que ejecute el HGS en el hardware físico aislado del resto de la infraestructura, o en tejidos de virtualización independientes o suscripciones de Azure.

  - Windows Server 2019 Standard o Datacenter Edition.
  - 2 CPU
  - 8 GB DE RAM
  - almacenamiento de 100 GB

  Puede usar el canal de [mantenimiento a largo plazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o el [canal semianual](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar y descargar Windows Server Insider Preview, consulte [Introducción al programa Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Elija un nombre para el nuevo bosque de Active Directory creado por el servicio de protección de host. 
  HGS no debe estar unido al dominio corporativo existente y debe tener administradores independientes para administrarlo.   

- Firewall y reglas de enrutamiento para permitir el tráfico entrante HTTP (TCP 80) o HTTPS (TCP 443) en los nodos del servicio de protección de host desde: 

  - Los equipos que ejecutan SQL Server
  - Los equipos que ejecutan aplicaciones cliente de base de datos (como servidores Web) que emiten consultas de base de datos y usan Always Encrypted con enclaves seguro. 

### <a name="sql-server-host-machines"></a>SQL Server de máquinas host

- La instancia de SQL Server debe ejecutarse en un equipo que cumpla los requisitos siguientes:

  - Windows 
    - Windows 10 Enterprise, versión 1809  
    - Windows Server 2019 Datacenter (canal semianual), versión 1809
    - Windows Server 2019 Datacenter
  - Máquina física para producción o máquina virtual de generación 2 para pruebas (tenga en cuenta que Azure no admite máquinas virtuales de generación 2)
  - Requisitos generales que se indican en [requisitos de hardware y software para instalar SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  Puede usar el canal de [mantenimiento a largo plazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o el [canal semianual](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar y descargar Windows Server Insider Preview, consulte [Introducción al programa Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Requisitos específicos del modo de atestación elegido:
  - El **modo TPM** es el modo de atestación más fuerte y utilizará un módulo de plataforma segura (TPM) para validar criptográficamente que los equipos host son conocidos para su centro de seguridad (mediante un identificador único de cada TPM), ejecutando hardware y firmware de confianza. configuraciones (con una línea base de TPM) y ejecución de código de modo de usuario y kernel de confianza (mediante el control de aplicaciones de Windows Defender). El siguiente hardware es necesario para usar el modo TPM: 
    - Módulo TPM 2,0 instalado y habilitado 
    - Arranque seguro habilitado con la Directiva de arranque seguro de Microsoft (no habilite la Directiva de CA de arranque seguro de terceros o las directivas personalizadas)
    - IOMMU (Intel VT-d o AMD IOV) para evitar ataques de acceso directo a memoria 

  - El **modo de clave de host** usa un par de claves asimétricas (como las claves SSH) para identificar y autorizar los equipos host que desean ejecutar SQL Server. Este modo es más fácil de configurar y no tiene ningún requisito de hardware específico, pero no comprobará el software o el firmware que se ejecuta en los equipos host.  

Para comprobar si el TPM es compatible, ejecute los siguientes comandos en el equipo host en el que desea ejecutar SQL Server mediante Always Encrypted con enclaves seguro. "2,0" debe aparecer en la lista de SpecVersions compatibles para que pueda usar la atestación de TPM:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configuración del primer nodo HGS 

El servicio de protección de host funciona en una configuración de alta disponibilidad con un clúster de tres nodos. Se recomienda que configure un nodo completamente antes de agregar otros nodos. 

Ejecute todos los comandos siguientes en una sesión de PowerShell con privilegios elevados.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Para Always Encrypted con enclaves seguro, los equipos host que ejecutan SQL Server y máquinas que ejecutan las aplicaciones cliente de base de datos deben ponerse en contacto con HGS, aunque solo los equipos host requieren atestación.

4. Una vez que se reinicie el equipo, se instalará HGS y el servidor también será un controlador de dominio con Active Directory configurado. 
   Durante la configuración de Active Directory, la cuenta de administrador del equipo local se agrega al grupo Admins. del dominio y solo los miembros de este grupo pueden iniciar sesión en un controlador de dominio.
   Inicie sesión con la cuenta de administrador de dominio (por administrator@bastion.local ejemplo, o bastión. local\administrator) o cree otra cuenta de administrador de dominio para iniciar sesión y, a continuación, configure el servicio de atestación.
   Tendrá que elegir la atestación de TPM o de clave de host y ejecutar el comando correspondiente. 
   Para HgsServiceName, especifique el DNN que ha elegido.

   Para el modo TPM:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Para el modo de clave de host:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Asegúrese de que los equipos host que ejecutan SQL Server podrán resolver los nombres DNS de los nuevos miembros del dominio HGS mediante la configuración de un reenviador de los servidores DNS corporativos en el nuevo controlador de dominio de HGS. Si usa DNS de Windows Server, puede configurar un reenviador condicional mediante la ejecución de los siguientes comandos en una consola de PowerShell con privilegios elevados en un servidor DNS de su organización. Sustituya los nombres y las direcciones en la sintaxis de Windows PowerShell que se indica a continuación según sea necesario para su entorno. Después de agregar más nodos de HGS, vuelva a ejecutar este comando para agregarlos como servidores maestros.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configuración de nodos de HGS adicionales para implementaciones de producción

Para agregar nodos al clúster, ejecute los siguientes comandos en una sesión de PowerShell con privilegios elevados. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Establezca la resolución del cliente DNS para que apunte al servidor HGS principal para que pueda resolver el nombre de dominio de HGS. Si usa un servidor con experiencia de escritorio, puede hacerlo en el centro de redes y recursos compartidos. En Server Core, puede usar la herramienta **sconfig. exe** , la opción 8 o **set-DnsClientServerAddress** para establecer la dirección DNS. 

3. A continuación, se promoverá este servidor a un controlador de dominio. Necesitará credenciales de administrador de dominio para completar esta tarea y se le pedirá que escriba una contraseña del modo de reparación de servicios de directorio después de ejecutar el comando. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configuración de HGS para HTTPS 

De forma predeterminada, cuando se inicializa el servidor HGS, se configuran los sitios web de IIS para las comunicaciones solo HTTP.

> [!NOTE]
> La configuración de HTTPS mediante un certificado de servidor HGS conocido y de confianza es necesaria para evitar ataques de tipo "Man in the Middle" y, por tanto, se recomienda para las implementaciones de producción.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> En el caso de Always Encrypted con Secure enclaves, el certificado SSL debe ser de confianza en ambos equipos host que ejecutan SQL Server y los equipos que ejecutan aplicaciones cliente de base de datos deben ponerse en contacto con HGS. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Recopilar información de atestación de los equipos host

Una vez que HGS está configurado, debe configurarse con información de atestación de los equipos host para que sepa qué máquinas deben estar autorizadas para realizar cálculos confidenciales mediante Always Encrypted y VBS Secure enclaves. Estos pasos varían en función del modo de atestación que use. 

### <a name="collect-tpm-attestation-artifacts"></a>Recopilar artefactos de atestación de TPM 

Si usa el modo TPM, ejecute los siguientes comandos en una sesión de PowerShell con privilegios elevados en cada equipo host para instalar la compatibilidad con la atestación y recopilar la información que necesitará para registrarse con el servicio de protección de host. 

1. Para instalar el cliente HGS en el equipo host, instale la característica de host protegido, que también instalará Hyper-V. 
   Aunque no vaya a ejecutar máquinas virtuales en este equipo, el hipervisor es necesario para habilitar las características de seguridad basadas en la virtualización que aíslan VBS enclaves.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie el equipo cuando se le pida que complete la instalación de Hyper-V. 
3. Cree una directiva de integridad de código para restringir el software que se puede ejecutar en el sistema. 
   Cualquier directiva de control de aplicaciones de Windows Defender es suficiente. 
   Si solo está ejecutando software de Microsoft en el servidor, los siguientes comandos crearán una directiva de forma rápida. 
   La directiva estará en modo auditoría, lo que significa que registrará un evento sobre código no autorizado, pero no evitará que se ejecute.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender Application control tiene numerosas características que cubren una variedad de posturas de seguridad. 
   Si necesita permitir software que no sea de Microsoft o personalizar la directiva predeterminada, se trata de la [Guía de implementación de Windows Defender Application control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Compruebe que la seguridad basada en la virtualización se está ejecutando en el equipo con el siguiente comando. 
   Sabrá que se está ejecutando VBS si el campo DeviceGuardSecurityServicesRunning contiene "HypervisorEnforcedCodeIntegrity". 
   Si no se está ejecutando, descargue la [herramienta de preparación de Device Guard](https://www.microsoft.com/download/details.aspx?id=53337) y ejecute "DG_Readiness. PS1-enable-HVCI" para habilitarla.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Recopilar el identificador y la línea base del TPM:

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copie los archivos XML, tcglog y bin de C:\artifacts en el servidor HGS.
7. Si este es el primer host de TPM que va a agregar al servidor de HGS, deberá instalar los certificados raíz de TPM de confianza en cada servidor de HGS. 
   Siga las [instrucciones de la documentación de HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) para completar este paso.
8. En el servidor HGS, autorice a este host para que pase la atestación. 
   Los siguientes scripts suponen que los 3 archivos se copiaron en C:\temp en el servidor de HGS y que el nombre del equipo del servidor es "ServerA". 
   Ajuste las rutas de acceso y los nombres para que coincidan con su propio entorno. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Su primer servidor ya está listo para atestar. 
   En el equipo host, ejecute el siguiente comando para indicarle dónde debe atestiguarse (cambie el nombre DNS por el del clúster HGS; normalmente usará el nombre del servicio HGS combinado con el nombre de dominio HGS). 
   Si recibe un error HostUnreachable, asegúrese de que puede resolver y hacer ping a los nombres DNS de los servidores HGS. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. El resultado del comando anterior debe mostrar que AttestationStatus = Passed. Si no es así, vea [errores de atestación](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) para obtener instrucciones sobre cómo resolver el error.   
11. Repita los pasos 1-10 para cada equipo host. 
    Si usa hardware idéntico, no necesitará capturar una nueva Directiva de base de referencia o de CI para cada máquina. 
    La línea de base del primer servidor cubrirá todas las máquinas configuradas de forma idéntica y la Directiva de CI se puede volver a usar en varios equipos, siempre y cuando el software de Microsoft sea el único software del equipo.

### <a name="collecting-host-keys"></a>Recopilación de claves de host 

> [!NOTE] 
> La atestación de clave de host solo se recomienda para su uso en entornos de prueba. La atestación de TPM proporciona las garantías más fuertes que VBS enclaves procesando los datos confidenciales en SQL Server ejecutan código de confianza y las máquinas se configuran con la configuración de seguridad recomendada. 

Si decide configurar HGS en el modo de atestación de clave de host, deberá generar y recopilar las claves de cada máquina host y registrarlas con el servicio de protección de host. 

1. Para obtener el cliente HGS instalado en el equipo host, instale la característica host protegido, que también instalará Hyper-V. 
   Aunque no vaya a ejecutar máquinas virtuales en este equipo, el hipervisor es necesario para habilitar las características de seguridad basadas en la virtualización que aíslan los VBS enclaves que ejecutan consultas Always Encrypted. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie el equipo cuando se le pida que complete la instalación de Hyper-V.
3. Genere una clave de host única y exporte la clave pública resultante a un archivo. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   También puede especificar una huella digital si desea utilizar su propio certificado. 
   Esto puede ser útil si desea compartir un certificado entre varios equipos o usar un certificado enlazado a un TPM o a un HSM. A continuación se muestra un ejemplo de creación de un certificado enlazado a TPM (que impide que se robe y se use la clave privada en otra máquina y que solo requiera un TPM 1,2):

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copie la clave de host en el servicio de protección de host. 
5. Registre la clave de host con cualquier nodo de HGS con un nombre y una ruta de acceso relevantes para su entorno: 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Su primer servidor ya está listo para atestar. 
   En el equipo host, ejecute el siguiente comando para indicarle dónde debe atestiguarse (cambie el nombre DNS por el del clúster HGS; normalmente usará el nombre del servicio HGS combinado con el nombre de dominio HGS). 
   Si recibe un error HostUnreachable, asegúrese de que puede resolver y hacer ping a los nombres DNS de los servidores HGS.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. El resultado del comando anterior debe mostrar que AttestationStatus = Passed. 
   Si recibe un error HostUnreachable, significa que el equipo host no se puede comunicar con HGS. 
   Asegúrese de que la resolución de DNS está configurada entre el equipo host y los servidores HGS, y que puede hacer ping en los servidores. 
   Un error UnauthorizedHost indica que la clave pública no se registró en el servidor HGS; Repita los pasos 4 y 5 para resolver el error. 
   Si todo lo demás no funciona, ejecute Clear-HgsClientHostKey y repita los pasos 3-6.   

8. Repita los pasos 1-7 para cada servidor que vaya a ejecutar SQL Server Always Encrypted mediante VBS enclaves.     


