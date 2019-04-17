---
title: Hospedada Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 23e586c22ca14af7b02550e2fa1c9522e379170c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="hosted-windows-server-essentials"></a>Hospedada Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento incluye información que es específica de host va a implementar Windows Server Essentials en su laboratorio y ofrecer Windows Server Essentials como un servicio a sus clientes.  
  
## <a name="what-is-windows-server-essentials"></a>¿Qué es Windows Server Essentials?  
 Windows Server Essentials es una solución de pequeñas empresas entre local, que incorpora las tecnologías de producto de 64 bits de las mejores para ofrecer un entorno de servidor adecuado para la mayoría de las pequeñas empresas. Las siguientes tecnologías se incluyen en Windows Server Essentials.  
  
 **Sistema operativo del servidor:** tecnologías de producto de Windows Server 2012 proporcionan el núcleo de Windows Server Essentials. Para obtener más información, visita el [sitio Web de Windows Server 2012](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Protección de datos:** Windows Server Essentials aprovecha las nuevas características disponibles en Windows Server 2012 para proporcionar funciones de protección de datos mejorada. La [nueva característica de espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx) dinámicamente le permite agregar la capacidad de almacenamiento físico de diferentes unidades de disco duro, agregar unidades de disco duro y crear volúmenes de datos con los niveles especificados de resistencia. Windows Server Essentials puede realizar copias de seguridad de todo el sistema y restaura la reconstrucción completa del servidor de sí mismo, así como los equipos cliente conectados a la red? ahora con compatibilidad para volúmenes de más de 2 TB. Nuevo con Windows Server 2012, el [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) puede usarse para proteger los archivos y carpetas en un servicio de almacenamiento en la nube que administrados por Microsoft. Windows Server Essentials también centralmente administra y configura la característica de historial de archivos de los clientes de Windows 8.1, ayudando a los usuarios recuperen de sobrescribir o eliminados accidentalmente archivos sin necesidad de asistencia de administrador.  
  
 **En cualquier lugar acceso:** acceso Web remoto proporciona una experiencia de explorador optimizado para pantallas táctiles para acceder a aplicaciones y datos desde prácticamente cualquier lugar que tienes una conexión a Internet y el uso de casi cualquier dispositivo. Windows Server Essentials también proporciona una aplicación de Windows Phone actualizada y una nueva aplicación para Windows 8.1 cliente equipos, lo que permite a los usuarios de forma intuitiva conectarse, para buscar en y acceder a archivos y carpetas en el servidor. Archivos automáticamente son almacenar en caché para el acceso sin conexión y se sincronizan cuando esté disponible una conexión con el servidor. Windows Server Essentials activa la configuración de red privada virtual (VPN) en un proceso sencillo, controlada por el Asistente de unos pocos clics y simplifica la administración de acceso VPN para los usuarios. Los equipos cliente pueden sacar provecho de una conexión VPN para unir el entorno de Windows SBS sin necesidad de camino a la oficina de forma remota.  
  
 **Flexibilidad de carga de trabajo:** Windows Server Essentials se ha diseñado para permitir que los clientes la flexibilidad para elegir qué aplicaciones y servicios que se ejecutan en forma local y que se ejecutan en la nube. En versiones anteriores, Windows Small Business Server Standard incluye Exchange Server como un producto de componente, que agrega los gastos y la complejidad para los clientes que deseaba aprovechar los servicios de mensajería y colaboración en la nube. Con Windows Server Essentials, los clientes pueden aprovechar el mismo tipo de experiencia de administración integrado si eligen ejecutar una copia local de Exchange Server, suscribirse a un servicio de Exchange hospedado o suscribirse a Microsoft Office 365.  
  
 **Supervisión de estado:** Windows Server Essentials supervisa su propio estado y el estado de los equipos cliente que ejecutan Windows 8.1, Windows 7 y Mac OS X versión 10,5 y versiones posteriores. Estado y te avisará de problemas o problemas relacionados con las copias de seguridad del equipo, almacenamiento del servidor, poco espacio en disco y mucho más.  
  
 **Extensibilidad:** Windows Server Essentials se basa en el modelo de extensibilidad de Windows SBS 2011 Essentials, que permite a otros proveedores de software agregar funcionalidades y características para el producto principal y agrega un nuevo conjunto de web API de servicios. También mantiene la compatibilidad con las existentes [kit de desarrollo de software](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) y [complementos](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) creado para Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>¿Cómo se puede personalizar una imagen?  
 Consulte la [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), que es un proceso de sysprep estándar de Windows Server con pasos adicionales de personalización de Windows Server Essentials. Para finalizar la personalización, sigue las instrucciones de [crear una imagen personalizada Simple](https://technet.microsoft.com/en-us/library/jj200117) y [personalizar la imagen](https://technet.microsoft.com/en-us/library/jj200161)y, a continuación, sigue las instrucciones en [preparación de la imagen para la implementación](https://technet.microsoft.com/en-us/library/jj200142) para capturar la imagen final.  
  
 Deberían prestar atención a los siguientes puntos:  
  
1.  Debe omitir la configuración inicial (CI) mediante la adición de un archivo SkipIC.txt a la raíz de todas las unidades. Después de instalar el servidor, antes de IC, presiona MAYÚS + F10 para iniciar la ventana cmd y crear un archivo de SkipIC.txt en la unidad C: / de unidad. Después de personalización, debes olvidar eliminar el archivo SkipIC.txt.  
  
2.  Si es necesario implementar Windows Server Essentials en un disco menor que 90 GB, debes agregar una clave del registro antes de sysprep:  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Después de sysprep, puedes usar la imagen de disco ejecuta Sysprep o reseal colocarlo en Install.wim para la implementación nuevo.  
  
 Si estás usando Virtual Machine Manager, puedes crear una plantilla con la instancia de ejecución. Crear una plantilla te la instancia de sysprep y apagar el servidor. Después de que se almacena en la biblioteca, puede hacer que aparezca la instancia en el caso por caso.  
  
##  <a name="BKMK_automatedeployment"></a>¿Cómo se puede automatizar la implementación?  
 Después de obtener la imagen personalizada, puedes hacer la implementación con tu propia imagen. Para realizar la instalación semiunattended, deberás proporcionar e implementación unattend.xml para la instalación de WinPE. Para realizar una instalación completamente desatendida, también debes proporcionar el archivo cfg.ini para la configuración inicial de Windows Server Essentials.  
  
1.  Realizar la instalación desatendida de WinPE. Esto automatizar solo la instalación de WinPE y permitir que la instalación detener antes de la configuración inicial para que los usuarios finales puedan proporcionar información Corp, dominio y administrador por sí mismos después RDP en la sesión del servidor. Para ello:  
  
    1.  Proporcionar el archivo unattend.xml de Windows. Sigue la [ADK de Windows 8.1](https://go.microsoft.com/fwlink/?LinkId=248694) para generar el archivo y proporcionar toda la información necesaria como nombre del servidor, claves de producto y contraseña de administrador. En la sección Microsoft-Windows-Shell-Setup del archivo unattend.xml, proporcionar la información como la siguiente.  
  
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
  
    2.  Puerto RDP 3389 debe abrirse en una dirección IP pública para que el cliente puede usar el administrador y la contraseña especificada en el archivo unattend.xml para RDP en el servidor para finalizar la configuración inicial.  
  
    > [!NOTE]
    >  Si no cambias la contraseña predeterminada, se detendrá la instalación del servidor en una pantalla pide una contraseña. **Nota** a los usuarios finales debe usar la cuenta de administrador predeterminada para iniciar sesión en el servidor y realizar la configuración inicial.  
  
 Si estás usando Virtual Machine Manager, puedes especificar la contraseña de administrador en la consola cuando se crea una nueva instancia de la plantilla.  
  
1.  Realizar completar la instalación desatendida incluida la configuración inicial de instalación desatendida. Para ello:  
  
    1.  Proporcionar el archivo unattend.xml igual que hicimos anteriormente, si la implementación se inicia desde el programa de instalación de WinPE.  
  
    2.  Consulta la sección de Windows Server Essentials ADK derecho, [crear el archivo Cfg.ini](https://technet.microsoft.com/en-us/library/jj200150), para generar el cfg.ini.  
  
    3.  Proporcionar información en [InitialConfiguration].  
  
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
  
    4.  Proporcionar información en [PostOSInstall].  
  
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
  
    5.  Si proporcionas el parámetro WebDomainName, asegúrate de que el registro de DNS también se está actualizando para que apunte a la dirección IP pública del servidor s.  
  
    6.  Si no has proporcionado la información de WebDomainName anterior, asegúrese de abrir puerto 3389 para que los clientes pueden usar RDP para conectarse al servidor y finalizar las configuraciones de VPN.  
  
> [!NOTE]
>  Asegúrate de que la configuración de zona horaria del servidor de host de máquina virtual y la máquina virtual de Windows Server Essentials es el mismo. De lo contrario, es posible que Notes varios errores diferentes (configuración inicial puede presentar errores en tareas relacionadas con el certificado; certificado puede no funcionar para unas pocas horas después de la instalación; no se actualizará la información del dispositivo correctamente, y así sucesivamente).  
  
 Después de la implementación, consulta la siguiente clave del registro en HKLM\software\microsoft\windows server\setup para comprobar si la configuración inicial se realizó correctamente. Si SetupStage == ICDone & & ICStatus == 1, que significa que la configuración inicial terminado correctamente.  
  
## <a name="what-is-the-supported-network-topology"></a>¿Qué es la topología de red compatibles?  
 Para usar Windows Server Essentials desde un cliente móvil, debe habilitarse la VPN. Te recomendamos habilitar el puerto 443 para conexiones VPN SSTP. Si la característica de servidor multimedia es necesaria para las aplicaciones de acceso Web remoto o un servicio Web, también debe habilitarse el puerto 80.  
  
 Habilitar VPN puede realizarse durante la implementación de instalación desatendida a través de la secuencia de comandos de Windows PowerShell o puede configurarse con nuestro Asistente tras la configuración inicial.  
  

-   ¿Para habilitar VPN durante la implementación de instalación desatendida, consulta [cómo se puede automatizar la implementación? ](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) en este documento.  

-   ¿Para habilitar VPN durante la implementación de instalación desatendida, consulta [cómo se puede automatizar la implementación? ](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) en este documento.  

  
-   Para habilitar la VPN a través de Windows PowerShell, ejecuta el siguiente cmdlet con privilegios administrativos y proporcionar toda la información necesaria.  
  
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
  
 Si no puede proporcionar una conexión VPN antes de dar el servidor a los clientes, asegúrate de que el puerto del servidor 3389 es accesible en Internet para que los usuarios finales pueden usar RDP para conectarse al servidor y realiza la configuración de por sí mismos.  
  
 Estas son las dos topologías típicas de red del servidor, y cómo se puede configurar el acceso Web VPN/control remoto (RWA):  
  
-   Topología 1 (preferido)  
  
    -   Servidor en una red virtual independiente en un dispositivo NAT.  
  
    -   Servicio DHCP está habilitado en la red virtual, o se asigna al servidor con una dirección IP estática.  
  
    -   Puerto 443 del servidor sea accesible desde pública IP puerto 443.  
  
    -   Se permite el paso VPN para el puerto 443.  
  
    -   Conjunto de direcciones IPv4 VPN se debe iban en la misma subred de la dirección del servidor.  
  
    -   Los servidores de segundo se deben asignar una dirección IP estática de la misma subred, que fuera el conjunto de direcciones VPN.  
  
-   Topología de 2:  
  
    -   El servidor tiene una dirección IP privada.  
  
    -   El puerto 443 en el servidor sea accesible desde un puerto de s de dirección IP público 443.  
  
    -   Se permite el paso VPN para el puerto 443.  
  
    -   Conjunto de direcciones IPv4 VPN está en un intervalo de la dirección del servidor diferente.  
  
 Con topología 2, no se admiten escenarios de servidor de segundo.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Cómo realizar tareas comunes a través de Windows PowerShell  
 **Habilitar el acceso Web remoto**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Ejemplo:  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 Este comando habilitar el acceso Web remoto con el enrutador configurado automáticamente y cambiar los permisos de acceso de forma predeterminada para todos los usuarios existentes.  
  
 **Agregar usuario**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 Ejemplo:  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 Este comando agrega un administrador denominado User2Test con contraseña Passw0rd!.  
  
 **Habilitar o deshabilitar el usuario**  
  
 Ejemplo:  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 Este comando deshabilitará al usuario denominado user2test. Valor UserStatus 1 a permitir que el usuario.  
  
 **Agregar la carpeta del servidor**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Ejemplo:  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Este comando agrega una carpeta del servidor denominada MyTestFolder en la ubicación especificada.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>¿Cómo se puede agregar un segundo servidor en el dominio de Windows Server Essentials?  
 Dado que Windows Server Essentials es un controlador de dominio, puedes unirte a un segundo servidor en el dominio de la manera estándar.  
  
## <a name="which-email-solutions-can-be-integrated"></a>¿Se pueden integrar las soluciones de correo electrónico?  
 Windows Server Essentials admite la integración con dos soluciones de correo electrónico sin necesidad de personalizarlos: Office 365 y Exchange local. Si estás ejecutando tu propia solución de correo electrónico hospedadas, necesitarás desarrollar un complemento para integrar Windows Server Essentials con la solución de correo electrónico hospedadas.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>¿Cómo se puede migrar local de Windows SBS (2008/2011/2003) a la hospedada de Windows Server Essentials?  
 Guías de migración están disponibles para local Windows Small Business Server (Windows SBS) para migraciones de Windows Server Essentials. Algunos de los pasos no exactamente el mismo pueden aplicarse a su entorno hospedado. Sin embargo, las tareas generales y las cargas de trabajo que se migrarán deben ser el mismo. Te recomendamos que consultes la [guías de migración](https://go.microsoft.com/fwlink/p/?LinkID=254292) y realizar personalizaciones necesarias en función de tu entorno de host.  
  
 Se recomienda poner el servidor de origen y el servidor de destino en la misma subred. Si esto no es posible, debes asegurarte:  
  
-   El nombre DNS interno del servidor de origen y el servidor de destino son accesibles por entre sí.  
  
-   Todos los puertos necesarios están abiertos.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>¿Cómo puedo actualizar Windows Server Essentials para Windows Server Standard?  
 Windows Server Essentials puedes actualizar a Windows Server Standard. Quitará los bloqueos y límites y agrega los paquetes que faltan en Windows Server Standard. Para obtener más información, [descargar el documento](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>¿Cuáles son las herramientas de administración y supervisión nativas?  
  
### <a name="group-policy-management"></a>Administración de directivas de grupo  
 Windows Server Essentials aprovecha la compatibilidad nativa con la directiva de grupo en Windows Server 2012 y proporciona la interfaz de usuario para configurar la configuración de seguridad y la redirección de carpetas.  
  
> [!NOTE]
>  En un entorno hospedado, si se habilita la redirección de carpetas de un perfil de usuario, puede aumentar el tiempo para los usuarios finales iniciar sesión cuando el tamaño de los datos es grande.  
  
### <a name="management-pack"></a>Paquete de administración  
 Módulo de administración de Windows Server Essentials ofrece supervisión función sobre el sistema de alertas de estado en Windows Server Essentials para ayudar a anfitriones administrar grandes cantidades de servidores de Windows Server Essentials dedicados a las empresas de pequeñas empresas diferentes. La supervisión en esta versión incluye solo las alertas críticas en el sistema.  
  
#### <a name="management-pack-scope"></a>Ámbito del paquete de administración  
 Este módulo de administración te ayuda a supervisar características específicas de Windows Server Essentials. No se supervisa características que son genéricas en el sistema operativo Windows Server 2012 Standard. Para supervisar Windows Server Essentials, debes usar el paquete de administración de Windows Server Essentials y la administración de paquete para Windows Server 2012 Standard.  
  
#### <a name="mandatory-configuration"></a>Configuración obligatoria  
 Los siguientes pasos deben realizarse antes de poder usar el módulo de administración:  
  
1.  Instalar al agente y configurar la confianza con los certificados de confianza. Dado que Windows Server Essentials está preconfigurada como un controlador de dominio y no puede tener la confianza con otros dominios o bosques, el agente de System Center Operations Manager debe instalarse en Windows Server Essentials y configurado confianza con el uso de certificados de servidor de administración.  
  
2.  Descargar el paquete de administración. Para supervisar Windows Server Essentials mediante el uso de Operations Manager 2007, primero debes descargar el [módulo de administración del sistema operativo de Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) desde el catálogo de paquete de administración.  
  
3.  Importa el archivo de paquete de administración. Si estás usando una versión localizada del paquete de administración, debes importar el archivo del paquete principal de administración y el paquete de idioma.  
  
#### <a name="files-in-this-monitoring-pack"></a>Archivos en este paquete de la supervisión  
 La supervisión de paquete para Windows Server Essentials incluye los siguientes archivos:  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   . MP Microsoft.Windows.Server.2012.Essentials. < locale\ >  
  
### <a name="back-up-and-restore"></a>Copias de seguridad y restauración  
 Windows Server Essentials te permite hacer una copia del servidor y cliente.  
  
#### <a name="back-up-the-server"></a>Hacer copia de seguridad del servidor  
 Windows Server Essentials admite dos formas de hacer copias de seguridad del servidor: copia de seguridad local y copia de seguridad externo.  
  
 **Copia de seguridad local** te permite realizar la copia de seguridad de nivel de bloque incremental de forma regular para un disco separado. Como un host, puede adjuntar un disco virtual a la máquina virtual de Windows Server Essentials y configurar copia de seguridad de servidor en este disco virtual. El disco virtual debe estar ubicado en un disco físico diferente que la máquina virtual de Windows Server Essentials.  
  
-   Si tienes otro mecanismo para hacer copia de seguridad de la máquina virtual de Windows Server Essentials y no desea que el usuario pueda ver la característica copia de seguridad de servidor nativa de Windows Server Essentials, podrías desactivarlo y quitar todos los interfaz de usuario relacionados desde el escritorio de Windows Server Essentials. Para obtener más información, consulta la sección de personalizar la copia de seguridad del servidor de la [documento ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
 **Copia de seguridad externo** permite periódicamente hacer una copia de datos del servidor a un servicio de nube. Puede descargar e instalar el Microsoft Azure copia de seguridad de integración módulo para Windows Server Essentials para aprovechar la copia de seguridad de Azure proporcionado por Microsoft.  
  
 Si los usuarios prefiere otro servicio de nube, debes:  
  
1.  Actualiza la interfaz de usuario del panel Windows Server Essentials para que se incluye un vínculo a tu servicio de nube preferido, en lugar del predeterminado de copia de seguridad de Azure. Para obtener más información, consulta el personalizar la sección de la imagen de la [documento ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  (Opcional) Desarrolla un complemento para el escritorio de Windows Server Essentials configurar y administrar el servicio de copia de seguridad de la nube.  
  
#### <a name="back-up-the-client"></a>Hacer copia de seguridad del cliente  
 Windows Server Essentials admite dos tipos de copia de seguridad de datos de cliente: copia de seguridad de todo el cliente y el historial de archivos.  
  
> [!NOTE]
>  Copia el cliente puede afectar al rendimiento porque los datos se transfieren desde el cliente al servidor a través de VPN.  
  
 **Copia de seguridad de cliente completa** depende de manera predeterminada para todos los dispositivos cliente conectados a la red de Windows Server Essentials. Copia todo el cliente (sistema y los datos) de forma incremental y admite desduplicación de datos. Los datos de copia de seguridad estarán en el servidor que ejecuta Windows Server Essentials. Un error al cliente puede obtener sus datos a un punto de copia de seguridad anterior. Se puede desactivar esta característica siguiendo los pasos de crear la sección Cfg.ini archivo de la [documento ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
 Algunas consideraciones para la copia de seguridad de cliente completa son:  
  
-   Rendimiento: copia de seguridad de cliente inicial podría llevar mucho tiempo debido a la cantidad de datos que se carguen.  
  
-   Estabilidad: a veces, la conexión a Internet no es estable en el lado del cliente. Copia de seguridad de cliente está diseñado para ser reanudables y el punto de control predeterminado es de 40 GB (la base de datos de copia de seguridad de cliente creará un punto de control cada vez que han sido una copia de seguridad de datos de 40 GB). Puede cambiar este valor a un número menor si esperas que la conexión a Internet no es confiable.  
  
    -   Para habilitar un trabajo de punto de control, en el servidor, Establece la clave del Registro HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs en 1.  
  
    -   Para cambiar el umbral de punto de control, en el cliente, cambiar HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold desde su valor predeterminado (40 GB).  
  
-   Cliente recuperación: como entorno de preinstalación de Windows no admite la conexión VPN, restauración de cliente de reconstrucción completa no es compatible.  
  
 **Historial de archivos** es una característica de Windows 8.1 para realizar copias de seguridad de datos de perfil (bibliotecas, escritorio, contactos, Favoritos) en un recurso compartido de red. En Windows Server Essentials, permitimos administración centralizada de la configuración del historial de archivos de todos los clientes de Windows 8.1 Unidos a Windows Server Essentials. Los datos de copia de seguridad se almacenan en el servidor que ejecuta Windows Server Essentials. Puedes desactivar esta característica siguiendo los pasos de crear la sección Cfg.ini archivo de la [documento ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
### <a name="storage-management"></a>Administración de almacenamiento  
 La [nueva característica de espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx) dinámicamente le permite agregar la capacidad de almacenamiento físico de diferentes unidades de disco duro, agregar unidades de disco duro y crear volúmenes de datos con los niveles especificados de resistencia. También puedes adjuntar un disco iSCSI a Windows Server Essentials para expandir su almacenamiento.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>¿Cuáles son los principales escenarios que debo probar?  
 Desde la perspectiva de hospedaje, se recomienda que pruebes los siguientes escenarios:  
  
 **Implementación del servidor**  
  
-   Implementar el servidor de Windows Server Essentials en el entorno de laboratorio.  
  
-   Personaliza la imagen de Windows Server Essentials según sea necesario.  
  
-   Automatiza la implementación de Windows Server Essentials con archivos de instalación desatendida y cfg.ini.  
  
-   Migrar local de Windows SBS a Windows Server Essentials hospedado.  
  
-   Actualización de Windows Server Essentials para Windows Server 2012.  
  
 **Configuración del servidor**  
  
-   Configurar el acceso de (VPN, acceso Web remoto, DirectAccess) en cualquier lugar.  
  
-   Configurar el almacenamiento y la carpeta del servidor.  
  
-   (Si procede) Configurar copia de seguridad del servidor, la copia de seguridad en línea, la copia de seguridad de cliente, historial de archivos.  
  
-   (Si procede) Configurar y administrar espacios de almacenamiento.  
  
-   (Si procede) Configurar la integración de solución de correo electrónico (Office 365, hospedado Exchange y así sucesivamente).  
  
-   (Si procede) Configurar el servidor multimedia.  
  
 **Administración del servidor**  
  
-   Administrar los usuarios.  
  
-   Configurar y recibir correo electrónico de notificación de alertas.  
  
-   BPA se ejecutan en el caso de error o advertencia.  
  
-   Configurar el paquete de supervisión de System Center.  
  
-   Configurar la recuperación del servidor, en el caso de daños.  
  
 **Experiencia del cliente**  
  
-   Implementación de cliente a través de internet (PC o Mac OS).  
  
-   Launchpad de uso en el cliente para acceder a la carpeta compartida.  
  
-   Activos de servidor de acceso a través de acceso Web remoto, desde distintos dispositivos (PC, teléfono, tableta).  
  
-   Mi aplicación de Server para Windows Phone.  
  
-   (Si procede) Historial de archivos, copia de seguridad de cliente y restauración (no BMR), la redirección de carpetas.  
  
-   (Si procede) Experiencia de integración de correo electrónico.  
  
## <a name="where-can-i-get-more-support"></a>¿Dónde puedo obtener soporte técnico más?  
 Puedes obtener documentos SDK y del ADK de los siguientes vínculos:  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 Puede notificar un error al equipo de la característica a través de Connect. Para generar registros, zip la carpeta en el servidor y los clientes de unir el servidor: C:\ProgramData\Microsoft\Windows Server\Logs.
