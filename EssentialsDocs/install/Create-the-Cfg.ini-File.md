---
title: Cree el archivo Cfg.ini
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 597f6349d96d29f06f06034504d5800e7e207eae
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623779"
---
# <a name="create-the-cfgini-file"></a>Cree el archivo Cfg.ini

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

El archivo cfg.ini se usa para automatizar una instalación del sistema operativo en el siguiente escenario:

-   Al comprobar la experiencia del usuario final con una imagen preinstalada en un equipo de destino, la sección Configuración inicial se utiliza para guiar al usuario en la instalación, ya sea en modo atendido o desatendido. Para ello, consulte [Cómo crear la sección de Configuración inicial](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).

##  <a name="create-the-initial-configuration-section"></a><a name="BKMK_CreateInit2"></a> Crear la sección de configuración inicial
 Use la sección Configuración inicial del archivo cfg.ini para recibir asistencia durante la instalación, ya sea en modo atendido o desatendido.

#### <a name="to-define-the-initial-configuration-section"></a>Para definir la sección de Configuración inicial

1.  Si existe, abra el archivo cfg.ini en el Bloc de notas; en caso contrario, cree un archivo nuevo.

2.  Agregue el texto siguiente para crear la sección ConfiguraciónInicial.

    ```

    [InitialConfiguration]
    ;Optional, display language can only be one of the installed language
    Language=en-us
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.
    ;Optional
    Locale=en-us
    ;Optional
    Country=US
    ;Optional
    Keyboard=0409:00000409
    AcceptEula=true
    ;This is only required on a server where an OEM EULA has been specified
    ;by using the OOBE.xml file
    AcceptOEMEula=true
    ;Optional. Example: My Company Name
    CompanyName=EnterCompanyName
    ServerName=EnterServerName
    ; Example: CONTOSO
    NetbiosName=EnterNetbiosDomainName
    ; Example: contoso.local
    DNSName=EnterDNSDomain
    ; Used to set the user name for the domain admin
    UserName=EnterDomainAdminUserName
    ;The password has to be strong and at least 8 characters
    PlainTextPassword=EnterAdminPassword
    ;. Used to set the user name for the domain standard user account. Ignored in migration mode.
    StdUserName=EnterDomainStandardUserName
    ;. The password for the domain standard user account has to be strong and at least 8 characters
    StdUserPlainTextPassword=EnterStandardUserPassword
    ;Controls the Watson and automatic update settings
    Settings=All or Updates or None
    WebDomainName=www.abc.com
    TrustedCertFileName=c:\cert\a.pfx
    TrustedCertPassword=Enteryourpassword
    EnableVPN=true
    EnableRWA=true
    IPv4DNSForwarder=<IPV4Address,IPV4Address,¦>
    IPv6DNSForwarder=<IPV6Address,IPV6Address,¦>
    VpnIPv4StartAddress=<IPV4Address>
    VpnIPv4EndAddress=<IPV4Address>
    VpnBaseIPv6Address=<IPV6Address>
    VpnIPv6PrefixLength=<number>
    ;All these section are optional.
     [PostOSInstall]
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.

    IsHosted=true
    StaticIPv4Address=<IPV4Address>
    StaticIPv4Gateway=<IPV4Address>
    StaticIPv4SubnetMask=<IPV4SubnetMask>
    StaticIPv6Address=<IPV6Address>
    StaticIPv6SubnetPrefixLength=<number>
    StaticIPv6Gateway=<IPV6Address>
    ClientBackupOn=true
    FileHistoryOn=true
    LaunchPadHiddenTasks=<Microsoft.LaunchPad.AdminDashboard,Microsoft.LaunchPad.Backup>

    ```

    > [!NOTE]
    >  No se proporcionan opciones para seleccionar otro idioma en la configuración inicial. Si se restablece el sistema, el idioma del sistema operativo será el que se instaló originalmente.

    |Nombre de parámetro|Descripción del parámetro|
    |--------------------|---------------------------|
    |*AcceptEula*|Indica que el usuario acepta los Términos de licencia de software de Microsoft. El valor puede ser verdadero o falso, pero la instalación solo se realizará cuando se especifica verdadero.|
    |*AcceptOEMEula*|(Opcional) Indica que el usuario acepta el contrato de licencia del asociado. El valor puede ser verdadero o falso. Este campo solo se requiere si el servidor se adquirió a través de un asociado que proporciona un contrato de licencia aparte.|
    |*Compañía*|(Opcional) Nombre de la empresa. El nombre de la empresa se usa para asociar el servidor con la empresa y personalizar los informes de la empresa. Puede tener hasta 254 caracteres|
    |*País*|(Opcional) Cadena que representa la región/país deseado. Ejemplo: US para Estados Unidos.|
    |*ServerName*|El nombre del servidor identifica de forma única el servidor en la red. El nombre del servidor debe cumplir los criterios siguientes:<br /><br /> -Puede tener una longitud de hasta 15 caracteres.<br /><br /> : Puede contener letras, números y guiones (-).<br /><br /> -No debe comenzar con un guión.<br /><br /> : No debe contener espacios.<br /><br /> -No debe contener solo números.<br /><br /> Ejemplo: ContosoServer.|
    |*DNSName*|Un dominio interno agrupa el servidor y los equipos cliente para compartir una base de datos común de nombres de usuario, contraseñas y otros datos comunes. Los usuarios ven este nombre al iniciar sesión en sus equipos, pero solo se usa internamente y no coincide con el nombre de dominio de Internet. El nombre de dominio interno debe cumplir los mismos criterios especificados para *ServerName*.<br /><br /> Ejemplo: contoso.local.|
    |*NetbiosName*|Un nombre NetBIOS se usa para identificar recursos que se ejecutan en el servidor. Puede tener hasta 15 caracteres Ejemplo: Contoso.|
    |*Lenguaje*|(Opcional) Especifica el idioma que se mostrará. Solo puede ser uno de los idiomas instalados. Ejemplo: en-us para la variedad de inglés de los Estados Unidos.|
    |*Configuración regional*|(Opcional) Especifica el formato de hora y moneda con el formato *LocaleID*. Ejemplo: en-us para el formato de hora y moneda mostrado en inglés según los estándares usados en los Estados Unidos.|
    |*Teclado*|El teclado debe tener uno de los dos formatos siguientes:<br /><br /> - **idioma de entrada: distribución del teclado.** Por ejemplo, 0409:00000409, donde 0409 antes de **:** es el idioma de entrada y **00000409** es la disposición de teclado. Puede encontrar la lista de disposiciones de teclado en la clave del Registro **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts**.<br /><br /> - **idioma de entrada: el identificador de IME.** Abajo se muestra una lista completa de identificadores IME.<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {8F96574E-C86C-4bd6-9666-3F7327D4CBE8} método de entrada de amárico<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e} {FA550B04-5AD7-411F-A5AC-CA038EC515D7} Microsoft pinyin-simple rápido (Chino Simplificado)<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {B2F9C502-1742-11D4-9790-0080C882687E} chino (tradicional)-fonética nueva<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {4BDF9F03-C7D3-11D4-B2AB-0080C882687E} chino (tradicional)-ChangJie<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {6024B45F-5C54-11D4-B921-0080C882687E} chino (tradicional)-Quick<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {D38EFF65-AA46-4FD5-91A7-67845FB02F5B} matriz de chino tradicional<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {037B2C25-480C-4D7F-B027-D6CA6B69788A} chino tradicional DaYi<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36} {A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft IME (japonés)<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F} {B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft IME (Coreano)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F} {B60AF051-257A-46BC-B9D3-84DAD819BAFB} IME hangul antiguo (Coreano)<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {409C8376-007B-4357-AE8E-26316EE3FB0D} método de entrada Yi<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {3CAB88B7-CC3E-46A6-9765-B772AD7761FF} Tigrinya método de entrada|
    |*Configuración*|Especifica la selección del usuario para las actualizaciones. Utilice uno de los valores siguientes:<br /><br /> **-Todos los** equivalentes usan la configuración recomendada.<br /><br /> **-Actualizaciones es la instalación de** actualizaciones importantes. solo<br /><br /> **-Ninguno** es igual a no buscar actualizaciones.|
    |*UserName*|: El nombre de la nueva cuenta de administrador que se creó durante la instalación. El nombre de su cuenta de administrador y usuario estándar debe satisfacer los criterios siguientes:<br /><br /> -Puede tener una longitud de hasta 19 caracteres.<br /><br /> -No puede contener/\ []  &#124; < > + =; , ? *<br /><br /> -No debe comenzar ni terminar con un punto.<br /><br /> -No debe contener dos puntos consecutivos.<br /><br /> -No debe ser el mismo que el nombre del servidor o el nombre del dominio interno.<br /><br /> -No debe ser igual que un nombre de usuario predefinido, como administrador o invitado.|
    |*PlainTextPassword*|Se trata de la contraseña de la nueva cuenta de administrador que se creó durante la instalación.<br /><br /> -Debe tener una longitud de ocho caracteres como mínimo.<br /><br /> -Debe contener al menos tres de las cuatro categorías siguientes:<br /><br /> -Caracteres en mayúsculas.<br /><br /> -Caracteres en minúsculas.<br /><br /> Los.<br /><br /> Euro.|
    |*StdUserName*|El nombre de la nueva cuenta de usuario estándar que se creó durante la instalación. Consulte el parámetro *UserName* para conocer los requisitos.|
    |*StdUserPlainTextPassword*|La contraseña de la cuenta de usuario estándar que se creó durante la instalación.|
    |WebDomainName|(Opcional) Configure el nombre de dominio de Internet del servidor. Este archivo le permite configurar el nombre de dominio de forma similar al método utilizado para la configuración manual en el asistente de configuración de nombre de dominio.|
    |TrustedCertFileName|(Opcional) Configure un certificado de confianza para el nombre de dominio. Esto le permite añadir una certificación .PFX, que contiene la clave privada.|
    |TrustedCertPassword|(Opcional) Contraseña para importar el .PFX.|
    |EnableVPN|(Opcional) Activar la VPN de forma predeterminada.|
    |VpnIPv4StartAddress|(Opcional) Defina la dirección inicial de VPN.|
    |VpnIPv4EndAddress|(Opcional) Defina la dirección final de VPN.|
    |VpnBaseIPv6Address|(Opcional) Defina la dirección IPV6 de base para VPN.|
    |VpnIPv6PrefixLength|(Opcional) Defina la longitud del prefijo de la dirección IPv6 de VPN.|
    |IsHosted|(Opcional) El valor predeterminado es falso si no se ha especificado. Defina este valor si lo configura en un entorno administrado. Se deshabilitará la configuración del enrutador.|
    |StaticIPv4Address|(Opcional) Especifique la dirección IP estática si desea configurar una dirección IP estática en lugar de una dirección dinámica.|
    |StaticIPv4Gateway|(Opcional) Especifique la dirección de puerta de enlace predeterminada si desea configurar una dirección IP estática en lugar de una dirección dinámica.|
    |StaticIPv4SubnetMask|(Opcional) Especifique la máscara de subred si quiere configurar una dirección IP estática en lugar de una dirección dinámica.|
    |StaticIPv6Address|(Opcional) Especifique la dirección IP predeterminada si desea configurar una dirección IP estática en lugar de una dirección dinámica.|
    |StaticIPv6SubnetPrefixLength|(Opcional) Especifique la longitud predeterminada del prefijo de subred IPV6 si desea configurar una dirección IP estática en lugar de una dirección dinámica.|
    |StaticIPv6Gateway|(Opcional) Especifique una dirección predeterminada de la puerta de enlace si desea configurar una dirección IP estática en lugar de una dinámica.|
    |ClientBackupOn|(Opcional) Desactivar la copia de seguridad de servidor de forma predeterminada cuando nuevos clientes se unen al servidor.|
    |FileHistoryOn|(Opcional) Desactivar la copia de seguridad del historial de archivos cuando nuevos clientes con Windows 8 Consumer Preview se unen al servidor.|
    |EnableRWA|Habilitará el acceso Web remoto al instalar Windows Server Essentials, pero omitirá la configuración del enrutador. Esto solo se admite en una instalación limpia del producto. El valor predeterminado es false.|
    |IPv4DNSForwarder|Define el reenviador de DNS de las direcciones IPv4.|
    |IPv6DNSForwarder|Define el reenviador de DNS de las direcciones IPv6.|
    |LaunchPadHiddenTasks|-(Opcional) puede ocultar la entrada de copia de seguridad o la entrada del panel de administración en Launchpad.<br /><br /> -Para deshabilitar el panel: LaunchPadHiddenTasks = Microsoft. LaunchPad. AdminDashboard<br /><br /> -Para deshabilitar la copia de seguridad: LaunchPadHiddenTasks = Microsoft. LaunchPad. backup<br /><br /> -Para deshabilitar la copia de seguridad y el panel: LaunchPadHiddenTasks = Microsoft. LaunchPad. backup, Microsoft. LaunchPad. AdminDashboard|

3.  Guarde el archivo. Asegúrese de guardar el archivo como cfg.ini, y no como cfg.ini.txt.

    > [!NOTE]
    >  Puede guardar el archivo en una unidad flash USB, que puede utilizarse para fases específicas de la instalación, o bien copiar el archivo cfg.ini en el directorio raíz de cualquier disco duro en el servidor de destino. Debe asegurarse de que la codificación para el archivo se haya establecido como ANSI o Unicode, dado que la codificación UTF-8 no está admitida.

> [!IMPORTANT]
>  La sección Configuración inicial del archivo cfg.ini solo debe ser usada por el usuario final para personalizar el servidor o para que un asociado compruebe la experiencia del usuario del servidor al utilizar un archivo de respuestas desatendido. Esta sección del archivo no se utiliza para crear la imagen.

## <a name="see-also"></a>Consulte también

 [Introducción con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para](Preparing-the-Image-for-Deployment.md) [probar la implementación de la experiencia del cliente](Testing-the-Customer-Experience.md)

