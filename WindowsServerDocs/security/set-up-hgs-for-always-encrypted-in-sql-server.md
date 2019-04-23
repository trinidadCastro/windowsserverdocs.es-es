---
title: Configurar el servicio de protección de Host para Always Encrypted en SQL Server
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 2f800dfa01077287f8200dd8abea0be899776683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866696"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configurar el servicio de protección de Host para Always Encrypted con enclaves seguros en SQL Server 

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, vista previa de SQL Server 2019
 
[Always Encrypted con enclaves seguros](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) en SQL Server 2019 preview es una característica diseñada para habilitar confidenciales cálculos sobre datos confidenciales almacenados en una base de datos. El servicio de protección de Host (HGS) desempeña un papel importante en proteger los datos de un enclave seguro, configurado para Always Encrypted, una vez un enclave de memoria de la seguridad basada en virtualización (VBS). La seguridad de un enclave de memoria VBS depende de la seguridad del hipervisor de Windows y, en términos generales, la seguridad de la máquina que hospeda SQL Server. 

Por lo tanto, antes de que una aplicación de cliente de base de datos permite al enclave de memoria VBS usado para Always Encrypted para realizar cálculos en los datos confidenciales, debe fe de la aplicación con una confianza HGS. Certificación demuestra que el equipo que hospeda SQL Server, que contiene al enclave, está en el estado correcto y puede ser de confianza. Para el resto de este tema, nos referiremos a la máquina que hospeda SQL Server que simplemente la máquina host.

Hay dos maneras mutuamente excluyentes para la aplicación confirmar: 

- Atestación de clave de host autoriza un host demostrando posee una clave privada conocida y de confianza. Atestación de clave de host y un equipo host físico o una máquina virtual de generación 2 que ejecutan SQL Server se recomienda para entornos de preproducción.
- Atestación de TPM valida las mediciones de hardware para asegurarse de que un host ejecuta solo los archivos binarios correctos y directivas de seguridad. Atestación de TPM y un equipo host físico (no una máquina virtual) que ejecuta SQL Server, se recomienda para entornos de producción.

Para obtener más información sobre el servicio de protección de Host y puede medir, consulte [protegidos fabric y blindadas información general de las máquinas virtuales](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Tenga en cuenta que aunque los documentos hablan de las máquinas virtuales blindadas, las mismas protecciones, arquitectura y los procedimientos recomendados se aplican a SQL Server Always Encrypted con enclaves VBS. 

Este artículo le ayudará a configurar HGS en una configuración recomendada. 

## <a name="prerequisites"></a>Requisitos previos 

Esta sección describen los requisitos previos para las máquinas host y HGS. 

### <a name="hgs-servers"></a>Servidores HGS

- 1-3 servidores para ejecutar el HGS. 

  >[!NOTE]
  >Es necesario para un entorno de prueba o preproducción solo 1 servidor HGS.

  Estos servidores deben protegerse cuidadosamente, ya que controlan qué máquinas pueden ejecutar las instancias de SQL Server mediante Always Encrypted con enclaves seguros. 
  Se recomienda que distintos administradores administración el clúster HGS y ejecutar el HGS en hardware físico aislado del resto de la infraestructura, o en tejidos de virtualización independiente o las suscripciones de Azure.

  - Windows Server 2019 Standard o Datacenter edition.
  - 2 CPU
  - 8GB DE RAM
  - 100GB de almacenamiento

  Puede usar el [canal de mantenimiento a largo plazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o [canal semianual](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar y descargar Windows Server Insider Preview, consulte [introducción con el programa de Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Elija un nombre para el nuevo bosque de Active Directory creado por el servicio de protección de Host. 
  HGS no debe estar unido al dominio corporativo existente y debe tener distintos administradores administrarla.   

- Firewall y reglas de enrutamiento para permitir el tráfico HTTPS (443 de TCP) o HTTP de entrada (TCP 80) en los nodos del servicio guardián de Host desde: 

  - Los equipos que ejecutan SQL Server
  - Los equipos que ejecutan aplicaciones de cliente de base de datos (por ejemplo, servidores web) que emiten consultas de base de datos y usan Always Encrypted enclaves seguros. 

### <a name="sql-server-host-machines"></a>Máquinas host de SQL Server

- La instancia de SQL Server debe ejecutarse en un equipo que cumpla los requisitos siguientes:

  - Windows: 
    - Windows 10 Enterprise, versión 1809  
    - Windows Server 2019 Datacenter (canal semianual), versión 1809
    - Windows Server 2019 Datacenter
  - Máquina física para producción, o una máquina virtual de generación 2 para las pruebas (tenga en cuenta que Azure no admite máquinas virtuales de generación 2)
  - Requisitos generales que aparecen en [requisitos de Hardware y Software para instalar SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  Puede usar el [canal de mantenimiento a largo plazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o [canal semianual](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar y descargar Windows Server Insider Preview, consulte [introducción con el programa de Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Requisitos específicos para el modo de atestación elegido:
  - **Modo TPM** es el modo de atestación más fuerte y usará un módulo de plataforma segura (TPM) para validar que las máquinas host se sabe que su centro de datos (con un identificador único de cada TPM), ejecuta la confianza de hardware y firmware de forma criptográfica las configuraciones (mediante una línea de base TPM) y ejecución del núcleo de confianza y código en modo usuario (con Windows Defender Application Control). Para usar el modo TPM se requiere el siguiente hardware: 
    - Módulo TPM 2.0 instalado y habilitado 
    - Arranque seguro está habilitado con la directiva de arranque seguro de Microsoft (no habilitar la directiva del arranque seguro CA de confianza 3rd o las directivas personalizadas)
    - IOMMU (Intel VT-d o IOV AMD) para evitar ataques de acceso directo a la memoria 

  - **Modo de clave de host** utiliza un par de claves asimétricas (muy similar a las claves SSH) para identificar y autorizar a los equipos host que deseen ejecutar SQL Server. Este modo es más fácil de configurar y no tiene los requisitos de hardware específico, pero no comprobará el software o firmware que se ejecutan en los equipos host.  

Para comprobar si el TPM es compatible, ejecute los siguientes comandos en el equipo host donde se va a ejecutar SQL Server mediante Always Encrypted con enclaves seguros. "2.0" debe aparecer en la lista de admitidos SpecVersions para que pueda usar la atestación de TPM:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurar el primer nodo HGS 

El servicio de protección de Host funciona en una configuración de alta disponibilidad mediante un clúster de 3 nodos. Se recomienda que configure un nodo por completo antes de agregar otros nodos. 

Ejecutar todos los comandos siguientes en una sesión de PowerShell con privilegios elevados.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Always Encrypted con enclaves seguros, los equipos host que ejecutan SQL Server y equipos que ejecuten aplicaciones de cliente de base de datos que ambos deben ponerse en contacto con HGS, aunque solo los equipos host requieren la atestación.

4. Una vez reiniciado el equipo, se instalará HGS y el servidor también será un controlador de dominio con Active Directory configurado. 
   Durante la configuración de Active Directory, se agrega la cuenta de administrador del equipo local al grupo Admins. del dominio y solo los miembros de este grupo pueden iniciar sesión en un controlador de dominio.
   Inicie sesión con la cuenta de administrador de dominio (por ejemplo, administrator@bastion.local o bastion.local\administrator) o cree otra cuenta de administrador de dominio para iniciar sesión y, a continuación, configurar el servicio de atestación.
   Deberá elegir la atestación de clave de TPM o el host y ejecute el comando correspondiente. 
   Para el HgsServiceName, especifique la DNN que eligió.

   Para el modo TPM:
   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Para el modo de clave de host:
   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Asegúrese de que los equipos host que ejecutan SQL Server será capaces de resolver los nombres DNS de los miembros del dominio HGS nueva mediante la configuración de un reenviador de los servidores DNS corporativos para el nuevo controlador de dominio HGS. Si usa DNS de Windows Server, puede configurar un reenviador condicional mediante la ejecución de los siguientes comandos en una consola de PowerShell con privilegios elevados en un servidor DNS de su organización. Sustituya los nombres y direcciones en la sintaxis de Windows PowerShell a continuación según sea necesario para su entorno. Después de agregar más nodos HGS, ejecute este comando otra vez para agregarlos como servidores maestros.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurar nodos HGS adicionales para las implementaciones de producción

Para agregar nodos al clúster, ejecute los siguientes comandos en una sesión de PowerShell con privilegios elevados. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Establezca la resolución de cliente DNS para que apunte al servidor HGS principal para que pueda resolver el nombre de dominio HGS. Si usa el servidor con experiencia de escritorio, puede hacerlo en el centro de redes y recursos compartidos. En Server Core, puede usar el **sconfig.exe** herramienta, 8, la opción o **Set-DnsClientServerAddress** para establecer la dirección DNS. 

3. A continuación, se promoverá a este servidor a controlador de dominio. Necesitará las credenciales de administrador de dominio para completar esta tarea y le pedirá que escriba una contraseña de modo de reparación de servicios de directorio después de ejecutar el comando. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configuración de HGS para HTTPS 

De forma predeterminada, al inicializar el servidor HGS configurará los sitios web IIS para las comunicaciones de sólo HTTP.

>[!NOTE]
>Configurar HTTPS con un certificado de servidor HGS conocido y de confianza, es necesario para evitar ataques man-in-the-middle y, por tanto, se recomienda para las implementaciones de producción.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

>[!NOTE]
>Always Encrypted con enclaves seguros, el certificado SSL debe ser de confianza en ambos equipos host que ejecutan SQL Server y póngase en contacto con HGS máquinas que ejecuten aplicaciones de cliente de base de datos. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Recopilar información de atestación de los equipos host

Una vez configurado HGS, debe configurarse con la información de atestación de las máquinas host para que sepa qué equipos deben estar autorizados para realizar cálculos confidenciales con Always Encrypted y VBS enclaves seguros. Estos pasos varían en función de qué modo de atestación que use. 

### <a name="collect-tpm-attestation-artifacts"></a>Recopilar los artefactos de atestación de TPM 

Si está utilizando el modo TPM, ejecute los siguientes comandos en una sesión de PowerShell con privilegios elevados en cada equipo host para instalar la compatibilidad para la atestación y recopilar la información que necesitará para registrar con el servicio de protección de Host. 

1. Para instalar al cliente HGS en el equipo host, instale la característica de Host protegido, que también se instalará Hyper-V. 
   Aunque no va a ejecutar las máquinas virtuales en esta máquina, el hipervisor es necesario para habilitar las características de seguridad basada en virtualización que aislar a los enclaves VBS.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie el equipo cuando se le solicite para completar la instalación de Hyper-V. 
3. Crear una directiva de integridad de código para restringir qué software puede ejecutarse en el sistema. 
   Cualquier directiva de Windows Defender Application Control es suficiente. 
   Si sólo está ejecutando software de Microsoft en el servidor, los siguientes comandos crear rápidamente una directiva para usted. 
   La directiva estará en modo auditoría, lo que significa que se registrará un evento sobre código no autorizado, pero no mantendrá se ejecute.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender Application Control tiene numerosas características que abarcan una gran variedad de posturas de seguridad. 
   Si necesita permitir software ajeno a Microsoft o personalizar la directiva predeterminada, se la [Guía de implementación de Windows Defender Application Control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Compruebe que la seguridad basada en la virtualización se está ejecutando en el equipo con el siguiente comando. 
   Sabrá que se está ejecutando VBS si el campo DeviceGuardSecurityServicesRunning tiene "HypervisorEnforcedCodeIntegrity" en su lista. 
   Si no se está ejecutando, descargue el [herramienta de preparación de Device Guard](https://www.microsoft.com/download/details.aspx?id=53337) y ejecute "DG_Readiness.ps1-permitir - HVCI" para habilitarla.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```
5. Recopilar el identificador TPM y la línea de base:

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copie el xml, tcglog y archivos binarios de C:\artifacts a su servidor HGS.
7. Si este es el primer host TPM que se va a agregar a un servidor HGS, deberá instalar los certificados de raíz TPM de confianza en cada servidor HGS. 
   Siga el [orientación sobre la documentación de HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) para completar este paso.
8. En el servidor HGS, autorizar este host para pasar la atestación. 
   Los scripts siguientes se suponen el 3 de los archivos se copiaron en C:\temp en el servidor HGS y que el nombre del equipo del servidor es "Servidora". 
   Ajuste las rutas de acceso y nombres para que coincida con su propio entorno. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```
9. El primer servidor ya está listo para dar fe. 
   En el equipo host, ejecute el comando siguiente le indiquen dónde dar fe (cambio que del nombre DNS de su clúster HGS, normalmente, usará el nombre del servicio HGS combinada con el nombre de dominio HGS). 
   Si recibe un error HostUnreachable, asegúrese de que pueda resolver y haga ping a los nombres DNS de los servidores HGS. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. El resultado del comando anterior debe mostrar esa AttestationStatus = correcto. Si no es así, consulte [atestación errores](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) para obtener instrucciones sobre cómo resolver el error.   
11. Repita los pasos 1 al 10 para cada equipo host. 
    Si usas un hardware idéntico, no deberá capturar una nueva instantánea o una directiva de CI para cada máquina. 
    La línea de base desde el primer servidor cubrirá todas las máquinas configuradas del mismo modo, y la directiva de CI puede volver a usarse en varios equipos siempre y cuando el software de Microsoft es el único software en el equipo.

### <a name="collecting-host-keys"></a>Recopilar claves de Host 

>[!NOTE] 
>Solo se recomienda la atestación de clave de host para su uso en entornos de prueba. Atestación de TPM proporciona las garantías más fuertes que enclaves VBS procesar los datos confidenciales en SQL Server están ejecutando código de confianza y las máquinas están configuradas con la configuración de seguridad recomendada. 

Si decide configurar HGS en modo de atestación de clave de host, deberá generar y recopilar las claves de cada equipo host y registrarlas con el servicio de protección de Host. 

1. Para obtener al cliente HGS instalado en el equipo host, instale la característica de Host protegido, que también se instalará Hyper-V. 
   Aunque no va a ejecutar las máquinas virtuales en esta máquina, el hipervisor es necesario para habilitar las características de seguridad basada en virtualización que aislar a los enclaves VBS que ejecutan consultas Always Encrypted. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie el equipo cuando se le solicite para completar la instalación de Hyper-V.
3. Generar una clave de host único y exportar la clave pública resultante a un archivo. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   Como alternativa, puede especificar una huella digital si desea usar su propio certificado. 
   Esto puede ser útil si desea compartir un certificado en varias máquinas, o use un certificado enlazado a un HSM o de un TPM. Este es un ejemplo de cómo crear un certificado enlazado a TPM (que impide tener la clave privada robado y usarse en otra máquina y requiere un TPM 1.2):

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copie la clave de host para el servicio de protección de Host. 
5. Registre la clave de host con cualquier nodo HGS, con un nombre pertinente y la ruta de acceso para su entorno: 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. El primer servidor ya está listo para dar fe. 
   En el equipo host, ejecute el comando siguiente le indiquen dónde dar fe (cambio que del nombre DNS de su clúster HGS, normalmente, usará el nombre del servicio HGS combinada con el nombre de dominio HGS). 
   Si recibe un error HostUnreachable, asegúrese de que pueda resolver y haga ping a los nombres DNS de los servidores HGS.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. El resultado del comando anterior debe mostrar esa AttestationStatus = correcto. 
   Si se produce un error HostUnreachable, significa que el equipo host no puede comunicarse con HGS. 
   Asegúrese de que la resolución de DNS se ha configurado entre la máquina host y los servidores HGS, y que puede hacer ping a los servidores. 
   Un error UnauthorizedHost indica que la clave pública no se ha registrado con el servidor HGS, repita los pasos 4 y 5 para resolver el error. 
   Si todo lo demás falla, ejecute Clear-HgsClientHostKey y repita los pasos 3 a 6.   

8. Repita los pasos del 1 al 7 para cada servidor que se ejecutará SQL Server Always Encrypted con enclaves VBS.     


