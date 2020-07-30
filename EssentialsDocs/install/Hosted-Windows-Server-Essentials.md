---
title: Versión hospedada de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 55d4059361189a0117bfd197c030fb860a1b10bd
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181231"
---
# <a name="hosted-windows-server-essentials"></a>Versión hospedada de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este documento se incluye información específica de los proveedores de servicios de hosting que pretenden implementar Windows Server Essentials en su laboratorio y ofrecer Windows Server Essentials como servicio a sus clientes.

## <a name="what-is-windows-server-essentials"></a>¿Qué es Windows Server Essentials?
 Windows Server Essentials es una solución de pequeña empresa entre entornos, que incorpora las tecnologías de productos de 64 bits mejores para ofrecer un entorno de servidor adecuado para la gran mayoría de las pequeñas empresas. Las siguientes tecnologías se incluyen en Windows Server Essentials.

 **Sistema operativo del servidor:** Las tecnologías de productos de Windows Server 2012 proporcionan el núcleo de Windows Server Essentials. Para obtener más información, visite el [sitio web de Windows Server 2012](https://www.microsoft.com/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).

 **Protección de datos:** Windows Server Essentials aprovecha varias características nuevas disponibles en Windows Server 2012 para proporcionar capacidades de protección de datos considerablemente mejoradas. La [nueva característica Espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx) le permite agregar la capacidad de almacenamiento físico de unidades de disco duro separadas, agregar unidades de disco duro de forma dinámica y crear volúmenes de datos con niveles especificados de resistencia. Windows Server Essentials puede realizar copias de seguridad completas del sistema y restauraciones de reconstrucción completa del propio servidor, así como de los equipos cliente conectados a la red, ahora con compatibilidad con volúmenes mayores de 2 TB. Nuevo con Windows Server 2012, [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) se puede usar para proteger los archivos y las carpetas en un servicio de almacenamiento basado en la nube que administra Microsoft. Windows Server Essentials también administra y configura de forma centralizada la característica de historial de archivos de los clientes de Windows 8.1, ayudando a los usuarios a recuperarse de archivos eliminados o sobrescritos accidentalmente sin necesidad de ayuda del administrador.

 **Acceso desde cualquier lugar:** Acceso Web remoto proporciona una experiencia de exploración sencilla y compatible con funciones táctiles para el acceso a las aplicaciones y los datos desde prácticamente cualquier lugar con conexión a Internet y con casi cualquier dispositivo. Windows Server Essentials también proporciona una aplicación Windows Phone actualizada y una nueva aplicación para Windows 8.1 equipos cliente, lo que permite a los usuarios conectarse de forma intuitiva a los archivos y carpetas del servidor, buscar en ellos y acceder a ellos. Los archivos se almacenan también automáticamente en la memoria caché para el acceso sin conexión y se sincronizan cuando hay disponible una conexión al servidor. Windows Server Essentials convierte la configuración de redes privadas virtuales (VPN) en un proceso sencillo y controlado por asistente con tan solo unos clics, y simplifica la administración del acceso VPN para los usuarios. Los equipos cliente pueden aprovechar la conexión VPN para unirse de forma remota al entorno Windows SBS sin necesidad de viajar diariamente a la oficina.

 **Flexibilidad de la carga de trabajo:** Windows Server Essentials se ha diseñado para permitir a los clientes la flexibilidad de elegir qué aplicaciones y servicios se ejecutan de forma local y cuáles se ejecutan en la nube. En las versiones anteriores, Windows Small Business Server Standard incluía Exchange Server como producto componente, lo que añadía gastos y complejidad para los clientes que deseaban aprovechar los servicios de mensajería y colaboración basados en la nube. Con Windows Server Essentials, los clientes pueden aprovechar el mismo tipo de experiencia de administración integrada tanto si eligen ejecutar una copia local de Exchange Server, suscribirse a un servicio de Exchange hospedado o suscribirse a Microsoft Office 365.

 **Seguimiento de estado:** Windows Server Essentials supervisa su propio estado de mantenimiento y el estado de los equipos cliente que ejecutan Windows 8.1, Windows 7 y Mac OS X versión 10,5 y posteriores. El estado de mantenimiento le informa de los problemas relacionados con copias de seguridad del equipo, almacenamiento del servidor, bajo espacio en disco, etc.

 **Extensibilidad:** Windows Server Essentials se basa en el modelo de extensibilidad de Windows SBS 2011 Essentials, que permite a otros proveedores de software agregar funciones y características al producto principal, y agrega un nuevo conjunto de API de servicios Web. También mantiene la compatibilidad con el [kit de desarrollo de software](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) existente y con los [complementos](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) creados para Windows SBS 2011 Essentials.

## <a name="how-can-i-customize-an-image"></a>¿Cómo se personaliza una imagen?
 Consulte [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), que es un proceso Sysprep estándar de Windows Server con pasos de personalización de Windows Server Essentials adicionales. Para finalizar la personalización, siga las instrucciones que se indican en [Crear una imagen personalizada básica](https://technet.microsoft.com/library/jj200117) y [Personalizar la imagen](https://technet.microsoft.com/library/jj200161)y, a continuación, siga las instrucciones que se indican en [Preparar la imagen para su implementación](https://technet.microsoft.com/library/jj200142) para capturar la imagen final.

 Debe prestar atención a los siguientes puntos:

1. Deberá omitir la configuración inicial (IC) agregando un archivo SkipIC.txt a la raíz de cualquier unidad. Después de instalar el servidor, y antes de IC, presione Mayús+F10 para iniciar la ventana cmd y crear un archivo SkipIC.txt bajo la unidad C:/. Finalizada la personalización, no debe olvidar eliminar el archivo SkipIC.txt.

2. Si necesita implementar Windows Server Essentials en un disco inferior a 90 GB, debe agregar una clave del registro antes de Sysprep:

   ```
   %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f
   ```

   Después de sysprep, puede utilizar la imagen de disco con sysprep o volverla a sellar en Install.wim para realizar una nueva implementación.

   Si está utilizando Virtual Machine Manager, puede crear una plantilla mediante la instancia en ejecución. Al crear una plantilla, la instancia se preparará para el sistema y se apagará el servidor. Después de almacenarla en su biblioteca, puede utilizar la instancia caso por caso.

##  <a name="how-do-i-automate-the-deployment"></a><a name="BKMK_automatedeployment"></a>Cómo automatizar la implementación
 Después de obtener la imagen personalizada, puede realizar la implementación con su propia imagen. Para realizar una instalación semidesatendida, deberá proporcionar o implementar el archivo unattend.xml para la instalación de WinPE. Para realizar una instalación completamente desatendida, también debe proporcionar el archivo de cfg.ini para la configuración inicial de Windows Server Essentials.

1. Realice únicamente la instalación desatendida de WinPE. De esta manera se automatizará solamente la instalación de WinPE, y permitirá que la instalación se detenga antes de la configuración inicial de modo que los usuarios finales puedan proporcionar por sí mismos información sobre la corporación, el dominio y el administrador después de RDP en la sesión de servidor. Para ello, siga estos pasos:

   1.  Proporcione el archivo unattend.xml de Windows. Siga el [Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) para generar el archivo y proporcione toda la información necesaria, como el nombre del servidor, las claves de producto y la contraseña de administrador. En la sección Microsoft-Windows-Setup del archivo de unattend.xml, proporcione la información que aparece a continuación.

       ```
       <InstallFrom>
                <MetaData>
                    <Key>IMAGE/WINDOWS/EDITIONID</Key>
                    <Value>ServerSolution</Value>
                </MetaData>
                <MetaData>
                    <Key>IMAGE/WINDOWS/INSTALLATIONTYPE</Key>
                    <Value>Server</Value>
                </MetaData>
          </InstallFrom>
       ```

   2.  El puerto RDP 3389 debe estar abierto en una dirección IP pública para que el cliente pueda usar el administrador y la contraseña especificada en el archivo de unattend.xml en RDP en el servidor para finalizar la configuración inicial.

   > [!NOTE]
   >  Si no cambia la contraseña predeterminada, la instalación del servidor se detendrá en una pantalla que le pide que escriba una contraseña.**Nota:** los usuarios finales deben usar la cuenta de administrador predeterminada para iniciar sesión en el servidor y realizar la configuración inicial.

   Si va a utilizar Virtual Machine Manager, puede especificar la contraseña del administrador en la consola cuando cree una nueva instancia a partir de la plantilla.

2. Realice la instalación desatendida completa, incluida la configuración inicial desatendida. Para ello, siga estos pasos:

   1.  Proporcione el archivo unattend.xml como hizo anteriormente, si la implementación se inicia a partir de la instalación de WinPE.

   2.  Consulte la sección del ADK de Windows Server Essentials titulada [crear el archivo de Cfg.ini](https://technet.microsoft.com/library/jj200150)para generar el cfg.ini.

   3.  Proporcione información en [InitialConfiguration].

       ```
       WebDomainName=yourdomainname
       TrustedCertFileName=c:\cert\a.pfx
       TrustedCertPassword=Enteryourpassword
       EnableVPN=true
       EnableRWA=true
       ; Provide all information so that after setup is complete, your customer can use your domain name to visit the server directly with the admin/user information you provide in the [InitialConfiguration] section.

       VpnIPv4StartAddress=<IPV4Address>
       VpnIPv4EndAddress=<IPV4Address>
       VpnBaseIPv6Address=<IPV6Address>
       VpnIPv6PrefixLength=<number>
       ; Provide this information. IPv4StartAddress and IPv4Endaddress are required so that your VPN client can acquire valid IP through this range.

       IPv4DNSForwarder=<IPV4Address,IPV4Address,Â¦>
       IPv6DNSForwarder=<IPV6Address,IPV6Address,Â¦>
       ; Provide this information as needed according to your network environment settings.
       ```

   4.  Proporcione información en [PostOSInstall].

       ```
       IsHosted=true
       ; Must have, this will prevent Initial Configure webpage available for other computers under same subnet.

       StaticIPv4Address=<IPV4Address>
       StaticIPv4Gateway=<IPV4Address>
       StaticIPv6Address=<IPV6Address>
       StaticIPv6SubnetPrefixLength=<number>
       StaticIPv6Gateway=<IPV6Address>
       ; All these are optional if you have DHCP Server Service on the subnet, otherwise provide static IP here.
       ```

   5.  Si proporciona el parámetro WebDomainName, asegúrese de que el registro DNS también se está actualizando para que apunte a la dirección IP pública del servidor.

   6.  Si no ha proporcionado la información anterior sobre WebDomainName, asegúrese de abrir el puerto 3389 para que los clientes puedan utilizar RDP para conectarse al servidor y finalizar las configuraciones de VPN.

> [!NOTE]
>  Asegúrese de que la configuración de zona horaria del servidor host de máquina virtual y la máquina virtual de Windows Server Essentials es la misma. De lo contrario, podría experimentar diversos errores (la configuración inicial puede dar error en las tareas relacionadas con los certificados, puede que los certificados no funcionen durante algunas horas tras la instalación, que la información de los dispositivos no se actualice correctamente, etc.).

 Tras la implementación, compruebe la siguiente clave del Registro en HKLM\software\microsoft\windows server\setup para comprobar que la configuración inicial se haya realizado con éxito. Si SetupStage == ICDone && ICStatus == 1, significa que la configuración inicial ha finalizado correctamente.

## <a name="what-is-the-supported-network-topology"></a>¿Cuál es la topología de red admitida?
 Para usar Windows Server Essentials desde un cliente móvil, debe estar habilitada la VPN. Se recomienda habilitar el puerto 443 para las conexiones SSTP de VPN. Si la característica Servidor multimedia es necesaria para las aplicaciones de acceso Web remoto o de servicio web, también se debe habilitar el puerto 80.

 La habilitación de VPN se puede realizar durante la implementación desatendida a través de nuestro script de Windows PowerShell o se puede configurar con nuestro asistente tras la configuración inicial.


- Para habilitar VPN durante la implementación desatendida, consulte [How do I automate the deployment?](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) en este documento.

- Para habilitar VPN durante la implementación desatendida, consulte [How do I automate the deployment?](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) en este documento.


- Para habilitar VPN a través de Windows PowerShell, ejecute el siguiente cmdlet con privilegios administrativos y proporcione toda la información necesaria.

  ```
  ##
  ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).
  ##

  $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server
  $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file
  $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate
  $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate

  Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';
  [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);
  ##
  ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).
  ##

  Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;
  ```

  Si no puede proporcionar una conexión VPN antes de entregar el servidor a los clientes, asegúrese de que se pueda establecer comunicación con el puerto del servidor 3389 en Internet para que los usuarios finales puedan utilizar RDP para conectar con el servidor y realizar la configuración por sí mismos.

  A continuación se describen dos topologías típicas de red del lado del servidor, y cómo se podría configurar VPN/Acceso Web remoto (RWA):

- Topología 1 (preferida)

  -   El servidor en una red virtual independiente bajo un dispositivo NAT.

  -   El servicio DHCP está habilitado en la red virtual o el servidor tiene asignada una dirección IP estática.

  -   Se puede establecer comunicación con el puerto 443 desde el puerto 443 de la IP pública.

  -   Se permite el paso de VPN para el puerto 443.

  -   El grupo de direcciones IPv4 de VPN debe tener un rango en la misma subred de la dirección del servidor.

  -   A los servidores secundarios se les debe asignar una IP estática dentro de la misma subred, pero fuera del grupo de direcciones VPN.

- Topología 2:

  -   El servidor tiene una dirección IP privada.

  -   Se puede acceder al puerto 443 en el servidor desde una dirección IP pública, puerto 443.

  -   Se permite el paso de VPN para el puerto 443.

  -   El grupo de direcciones IPv4 de VPN está en un rango diferente de la dirección del servidor.

  Con la topología 2, no se admiten escenarios de servidores secundarios.

## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>¿Cómo se realizan tareas comunes a través de Windows PowerShell?
 **Habilitar el acceso Web remoto**

```
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]
```

 Ejemplo:

```
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers
```

 Este comando habilitará el acceso Web remoto con el enrutador configurado automáticamente y cambiará los permisos de acceso predeterminados de todos los usuarios existentes.

 **Agregar usuario**

```
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]
```

 Ejemplo:

```
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test
```

 Este comando agregará un administrador denominado User2Test con la contraseña Passw0rd!.

 **Habilitar/Deshabilitar usuario**

 Ejemplo:

```
$CurrentUser = get-wssuser  œname user2test
$CurrentUser.UserStatus = 0
$CurrentUser.Commit()
```

 Este comando deshabilitará el usuario llamado user2test. Al establecer UserStatus en 1 se habilitará el usuario.

 **Agregar carpeta de servidor**

```
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]
```

 Ejemplo:

```
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"
```

 Este comando agregará una carpeta de servidor denominada MyTestFolder en la ubicación especificada.

## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Cómo agregar un segundo servidor al dominio de Windows Server Essentials?
 Dado que Windows Server Essentials es un controlador de dominio, puede unir un segundo servidor al dominio de manera estándar.

## <a name="which-email-solutions-can-be-integrated"></a>¿Qué soluciones de correo electrónico se pueden integrar?
 Windows Server Essentials admite la integración con dos soluciones de correo electrónico integradas: Office 365 y Exchange local. Si está ejecutando su propia solución de correo electrónico hospedada, deberá desarrollar un complemento para integrar Windows Server Essentials con la solución de correo electrónico hospedada.

## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Cómo migrar Windows SBS (2011/2008/2003) local a la instancia hospedada de Windows Server Essentials
 Hay guías de migración disponibles para las migraciones de Windows Small Business Server (Windows SBS) locales a Windows Server Essentials. Puede que algunos de los pasos no se apliquen exactamente igual en el entorno hospedado. Sin embargo, las tareas generales y las cargas de trabajo que se van a migrar deben ser iguales. Se recomienda consultar las [guías de migración](https://go.microsoft.com/fwlink/p/?LinkID=254292) y realizar las personalizaciones necesarias de contrato con el entorno de hospedaje.

 Se recomienda colocar el servidor de origen y el servidor de destino en la misma subred. Si no fuera posible, tenga en cuenta las siguientes precauciones:

-   El nombre DNS interno del servidor de origen y del servidor de destino deben de poder comunicarse.

-   Todos los puertos necesarios están abiertos.

## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>¿Cómo puedo actualizar Windows Server Essentials a Windows Server Standard?
 Puede actualizar Windows Server Essentials a Windows Server Standard. Elimine bloqueos y límites y agregue los paquetes que faltan desde Windows Server Standard. Para obtener más información, [descargue el documento](https://go.microsoft.com/fwlink/p/?LinkID=253181).

## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>¿Cuáles son las herramientas nativas para la supervisión y la administración?

### <a name="group-policy-management"></a>Administración de directivas de grupo
 Windows Server Essentials aprovecha la compatibilidad con directiva de grupo nativa en Windows Server 2012 y proporciona la interfaz de usuario para configurar la redirección de carpetas y la configuración de seguridad.

> [!NOTE]
>  En un entorno hospedado, si está habilitada la redirección de carpetas para un perfil de usuario, los usuarios finales podrían tardar más en iniciar sesión cuando el tamaño de los datos es grande.

### <a name="management-pack"></a>Módulo de administración
 El módulo de administración de Windows Server Essentials proporciona funciones de supervisión a través del sistema de alertas de estado en Windows Server Essentials para ayudar a los proveedores de hospedaje a administrar un gran número de servidores de Windows Server Essentials dedicados a diferentes empresas de pequeñas empresas. En esta versión la supervisión incluye solamente alertas críticas del sistema.

#### <a name="management-pack-scope"></a>Ámbito del módulo de administración
 Este módulo de administración le ayuda a supervisar características específicas de Windows Server Essentials. No supervisa aquellas que son genéricas del sistema operativo Windows Server 2012 Standard. Para supervisar Windows Server Essentials, debe usar el módulo de administración de Windows Server Essentials y el módulo de administración para Windows Server 2012 Standard.

#### <a name="mandatory-configuration"></a>Configuración obligatoria
 Antes de poder usar el módulo de administración, es necesario realizar los siguientes pasos:

1.  Instale el Agente y configure la confianza mediante el certificado de confianza. Dado que Windows Server Essentials está preconfigurado como controlador de dominio y no puede tener confianza con otros dominios o bosques, el agente de System Center Operations Manager debe instalarse en Windows Server Essentials y configurar la confianza con el servidor de administración mediante certificados.

2.  Descargue el módulo de administración. Para supervisar Windows Server Essentials mediante Operations Manager 2007, primero debe descargar el [módulo de administración del sistema operativo Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) desde el catálogo de módulos de administración.

3.  Importe el archivo del módulo de administración. Si va a utilizar una versión localizada del módulo de administración, deberá importar el archivo principal del módulo de administración y el paquete de idioma.

#### <a name="files-in-this-monitoring-pack"></a>Archivos de este módulo de administración
 El módulo de supervisión de Windows Server Essentials incluye los siguientes archivos:

-   Microsoft.Windows.Server.2012.Essentials.mp

-   Microsoft. Windows. Server. 2012. Essentials. <configuración regional \> . MP

### <a name="back-up-and-restore"></a>Copia de seguridad y restauración
 Windows Server Essentials le permite realizar una copia de seguridad del servidor y del cliente.

#### <a name="back-up-the-server"></a>Copia de seguridad del servidor
 Windows Server Essentials admite dos maneras de realizar copias de seguridad del servidor: copia de seguridad local y copia de seguridad local.

 La **copia de seguridad local** le permite realizar copias de seguridad incrementales a nivel de bloque de forma periódica en un disco aparte. Como anfitrión, podría conectar un disco virtual a la máquina virtual de Windows Server Essentials y configurar la copia de seguridad del servidor en este disco virtual. El disco virtual debe estar ubicado en un disco físico diferente al de la máquina virtual de Windows Server Essentials.

- Si tiene otro mecanismo para realizar una copia de seguridad de la máquina virtual de Windows Server Essentials y no desea que el usuario vea la característica de copia de seguridad del servidor nativa de Windows Server Essentials, puede desactivarla y quitar toda la interfaz de usuario relacionada del panel de Windows Server Essentials. Para obtener más información, consulte la sección personalizar la copia de seguridad del servidor del [documento de ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).

  La **copia de seguridad no local** le permite realizar copia de seguridad periódica de los datos del servidor en un servicio de nube. Puede descargar e instalar el módulo de integración de Microsoft Azure Backup para Windows Server Essentials para aprovechar las Azure Backup proporcionadas por Microsoft.

  Si usted o sus usuarios prefieren otro servicio de nube, debe:

1.  Actualice la interfaz de usuario del panel de Windows Server Essentials para que proporcione un vínculo a su servicio en la nube preferido, en lugar de a la Azure Backup predeterminada. Para obtener más información, consulte la sección Personalización de la imagen del [documento de ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).

2.  Opta Desarrolle un complemento para el panel de Windows Server Essentials para configurar y administrar el servicio de copia de seguridad en la nube.

#### <a name="back-up-the-client"></a>Copia de seguridad del cliente
 Windows Server Essentials admite dos tipos de copia de seguridad de datos de cliente: copia de seguridad completa del cliente e historial de archivos.

> [!NOTE]
>  La realización de una copia de seguridad del cliente podría afectar al rendimiento dado que los datos se han de transferir del cliente al servidor a través de VPN.

 La **copia de seguridad completa del cliente** está activada de forma predeterminada para todos los dispositivos cliente conectados a la red de Windows Server Essentials. Realiza copia de seguridad del cliente completo (sistema y datos) de manera incremental y admite la desduplicación de los datos. Los datos de copia de seguridad estarán en el servidor que ejecuta Windows Server Essentials. Cuando un cliente experimente errores, podrá devolver sus datos a un punto de copia de seguridad anterior. Esta característica se puede desactivar siguiendo los pasos descritos en la sección creación del archivo de Cfg.ini del [documento de ADK](https://technet.microsoft.com/library/jj200150).

 Algunos aspectos a tener en cuenta para la copia de seguridad completa del cliente:

- Rendimiento: la copia de seguridad inicial del cliente podría llevar tiempo debido a la cantidad de datos que se van a cargar.

- Estabilidad: en ocasiones la conexión a Internet no es estable en el lado del cliente. La copia de seguridad del cliente está concebida para poderse reanudar, y el punto de control predeterminado es 40 GB (la base de datos de copia de seguridad del cliente creará un punto de control cada vez que se realice copia de seguridad de 40 GB de datos). Este valor se puede cambiar por un número más pequeño si cree que la conexión a Internet no es muy confiable.

  -   Para permitir un trabajo de punto de control, en el servidor, establezca la clave del Registro HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs en 1.

  -   Para cambiar el umbral del punto de control, en el cliente, cambie el valor predeterminado (40 GB) de HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold.

- Restauración completa del cliente: como el entorno de preinstalación de Windows no admite conexiones VPN, no es posible la restauración completa del cliente.

  El **historial de archivos** es una característica Windows 8.1 para la copia de seguridad de datos de perfil (bibliotecas, escritorio, contactos, favoritos) en un recurso compartido de red. En Windows Server Essentials, se permite la administración central de la configuración del historial de archivos de todos los clientes de Windows 8.1 Unidos a Windows Server Essentials. Los datos de la copia de seguridad se almacenan en el servidor que ejecuta Windows Server Essentials. Puede desactivar esta característica siguiendo los pasos descritos en la sección creación del archivo de Cfg.ini del [documento de ADK](https://technet.microsoft.com/library/jj200150).

### <a name="storage-management"></a>Administración del almacenamiento
 La [nueva característica Espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx) le permite agregar la capacidad de almacenamiento físico de unidades de disco duro separadas, agregar unidades de disco duro de forma dinámica y crear volúmenes de datos con niveles especificados de resistencia. También puede conectar un disco iSCSI a Windows Server Essentials para expandir su almacenamiento.

## <a name="what-are-the-main-scenarios-i-should-test"></a>¿Cuáles son los principales escenarios que se deben probar?
 Desde la perspectiva del hospedaje, se recomienda probar los siguientes escenarios:

 **Implementación del servidor**

- Implementar el servidor de Windows Server Essentials en el entorno de laboratorio.

- Personalizar la imagen de Windows Server Essentials según sea necesario.

- Automatizar la implementación de Windows Server Essentials con archivos y cfg.ini desatendidas.

- Migrar Windows SBS local a Windows Server Essentials hospedado.

- Actualice de Windows Server Essentials a Windows Server 2012.

  **Configuración del servidor**

- Configurar Acceso desde cualquier lugar (VPN, Acceso Web remoto, DirectAccess).

- Configurar Almacenamiento y Carpeta de servidor.

- (Si es aplicable) Configurar Copia de seguridad del servidor, Online Backup, Copia de seguridad de cliente, Historial de archivos.

- (Si es aplicable) Configurar y administrar Espacios de almacenamiento.

- (Si es aplicable) Configurar la integración de las soluciones de correo electrónico (Office 365, Exchange hospedado, etc.).

- (Si es aplicable) Configurar el servidor multimedia.

  **Servidor de administración**

- administrar usuarios

- Configurar y recibir notificaciones de alertas por correo electrónico.

- Ejecutar BPA en caso de error o advertencia.

- Configurar el módulo de supervisión de System Center.

- Configurar la recuperación del servidor, en caso de daños.

  **Experiencia del cliente**

- Implementación del cliente a través de Internet (PC o Mac OS).

- Utilizar Launchpad en el cliente para tener acceso a las carpetas compartidas.

- Tener acceso a los activos del servidor a través de Acceso Web remoto y desde diferentes dispositivos (PC, teléfono, tableta).

- Aplicación Mi servidor para Windows Phone.

- (Si es aplicable) Historial de archivos, Copia de seguridad del cliente y Restauración (sin BMR), Redirección de carpetas.

- (Si es aplicable) Experiencia de integración del correo electrónico.

## <a name="where-can-i-get-more-support"></a>¿Dónde se puede obtener más ayuda?
 Puede obtener los documentos de SDK y ADK desde los siguientes vínculos:

- [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)

- [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)

  Puede notificar los errores al equipo de características a través de Conectar. Para generar registros comprima la carpeta en el servidor y en los clientes que se unen al servidor: C:\ProgramData\Microsoft\Windows Server\Logs.
