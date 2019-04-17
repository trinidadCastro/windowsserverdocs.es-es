---
title: Crear el archivo Cfg.ini
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 967db5f36ea27fb04eab9a6682a106ba0072d45d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-cfgini-file"></a>Crear el archivo Cfg.ini

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

El archivo cfg.ini se usa para automatizar la instalación del sistema operativo en el siguiente escenario:  
  
-   Al probar la experiencia del usuario final con una imagen preinstalado en el equipo de destino, la sección de la configuración inicial se usa para recorrer la instalación en un modo de instalación desatendida o atendida. Para ello, consulta [crear la sección de la configuración inicial](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).  
  
##  <a name="BKMK_CreateInit2"></a>Crear la sección de la configuración inicial  
 Usa la sección de la configuración inicial en el archivo cfg.ini para recorrer la instalación en un modo de instalación desatendida o atendida.  
  
#### <a name="to-define-the-initial-configuration-section"></a>Para definir la sección de la configuración inicial  
  
1.  Abre el archivo de cfg.ini en el Bloc de notas, si existe; de lo contrario, se crea un nuevo archivo.  
  
2.  Agrega el siguiente texto para crear una sección InitialConfiguration.  
  
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
    >  No se proporciona una opción para seleccionar un idioma diferente durante la configuración inicial. Si se restableció el sistema, el idioma del sistema operativo será aquella que se haya instalado originalmente.  
  
    |Nombre del parámetro|Descripción del parámetro|  
    |--------------------|---------------------------|  
    |*/ACCEPTEULA*|Indica que el usuario acepta los términos de licencia del Software de Microsoft. El valor puede ser igual a verdadero o falso, pero la instalación se realiza solo si se establece en True.|  
    |*AcceptOEMEula*|(Opcional) Indica que el usuario acepta el contrato de licencia del partner. El valor puede ser igual a True o False. Este campo es necesario solo si el servidor se ha comprado un partner que proporcionan un contrato de licencia independientes.|  
    |*CompanyName*|(Opcional) Nombre de la empresa. Nombre de tu empresa se usa para personalizar los informes de empresa y asociar el servidor de tu empresa. Puede tener hasta 254 caracteres.|  
    |*País*|(Opcional) Cadena que representa el país o región deseado. Ejemplo: Nosotros para Estados Unidos.|  
    |*Nombre de servidor*|El nombre del servidor identifica el servidor en la red. El nombre del servidor debe cumplir los siguientes criterios:<br /><br /> -Puede tener hasta 15 caracteres.<br /><br /> -Puede contener letras, números y guiones (-).<br /><br /> -No debe empezar con un guión.<br /><br /> -No se debe contener espacios.<br /><br /> -No se debe contener sólo números.<br /><br /> Ejemplo: ContosoServer.|  
    |*DNSName*|Un dominio interno agrupa los equipos servidor y cliente juntos para compartir una base de datos de nombres de usuario, las contraseñas y otra información común. Los usuarios verán este nombre cuando inician sesión en sus equipos, pero se usa interno solo y no es el mismo que un nombre de dominio de Internet. El nombre de dominio interno debe cumplir con los mismos criterios especificados para el *nombreDeServidor*.<br /><br /> Ejemplo: contoso.local.|  
    |*NombreNetBIOS*|Un nombre NetBIOS se usa para identificar los recursos que se ejecutan en el servidor. Puede ser un máximo de 15 caracteres. Ejemplo: Contoso.|  
    |*Idioma*|(Opcional) Especifica el idioma para mostrar. Solo puede ser uno de los idiomas instalados. Ejemplo: en-us para el inglés de Estados Unidos.|  
    |*Configuración regional*|(Opcional) Especifica el formato de hora y moneda mediante un *LocaleID* formato. Ejemplo: en-para la moneda y la hora mostrar en inglés y se da formato según los estándares que se usan en los Estados Unidos.|  
    |*Teclado*|El teclado puede estar en los dos formatos siguientes:<br /><br /> - **diseño de entrada de teclado: idioma.** Por ejemplo, 0409: 00000409, donde 0409 antes **:** es el idioma de entrada, y **00000409** es la distribución del teclado. Puedes encontrar la lista de distribución del teclado en la clave del registro **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard diseños**.<br /><br /> - **idioma de entrada: el identificador IME.** A continuación te mostramos una lista completa de los identificadores IME.<br /><br /> -Método de entrada Amárico {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8}<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} Pinyin de Microsoft - rápido Simple (chino simplificado)<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} chino (tradicional) - nuevo fonético<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} chino (tradicional) - ChangJie<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} chino (tradicional) - rápida<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B} matriz tradicional chino<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A} chino tradicional DaYi<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft IME (japonés)<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft IME (coreano)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} IME de Hangul antiguo (coreano)<br /><br /> -Método de entrada de Yi {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}<br /><br /> -Método de entrada Tigriña {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF}|  
    |*Configuración*|Establece la selección del usuario para las actualizaciones. Usa uno de los siguientes valores:<br /><br /> **-Todo** equivale a usar la configuración recomendada.<br /><br /> **: Actualiza** es igual a instalar las actualizaciones importantes. solo<br /><br /> **-Ninguno** igual no buscar actualizaciones.|  
    |*Nombre de usuario*|-El nombre de la nueva cuenta de administrador que se crea durante la instalación. El administrador y los nombres de cuenta de usuario estándar deben cumplir los siguientes criterios:<br /><br /> -Puede tener hasta 19 caracteres.<br /><br /> -No puede contener / \ [] & #124; < > + =;, ? *<br /><br /> -No debe iniciar o terminar con un punto.<br /><br /> -No se debe contener dos puntos seguidos.<br /><br /> -No se debe ser el mismo que el nombre del servidor o el nombre de dominio interno.<br /><br /> -No se debe ser el mismo que un nombre de usuario predefinida como administrador o invitado.|  
    |*PlainTextPassword*|Esta es la contraseña para la nueva cuenta de administrador que se crea durante la instalación.<br /><br /> -Debe ser al menos ocho caracteres.<br /><br /> -Debe contener al menos tres de las cuatro categorías siguientes:<br /><br /> -Mayúsculas.<br /><br /> -Minúsculas.<br /><br /> -Números.<br /><br /> -Símbolos.|  
    |*StdUserName*|El nombre de la nueva cuenta de usuario estándar que se crea durante la instalación. Consulta la *UserName* parámetro para conocer los requisitos.|  
    |*StdUserPlainTextPassword*|La contraseña de la cuenta de usuario estándar que se crea durante la instalación.|  
    |WebDomainName|(Opcional) Configurar el nombre de dominio de Internet del servidor. Este archivo te permite configurar el nombre de dominio similar al método usado para la configuración manual en el Asistente para configuración de nombre de dominio.|  
    |TrustedCertFileName|(Opcional) Configurar el certificado de confianza para el nombre de dominio. Esto te permite poner una. Certificado PFX, que contiene la clave privada.|  
    |TrustedCertPassword|(Opcional) La contraseña para importar el. PFX.|  
    |EnableVPN|(Opcional) Activar la VPN de manera predeterminada.|  
    |VpnIPv4StartAddress|(Opcional) Establecer la dirección de inicio de la VPN.|  
    |VpnIPv4EndAddress|(Opcional) Establecer la dirección de final de la VPN.|  
    |VpnBaseIPv6Address|(Opcional) Establecer la dirección IPV6 base para VPN.|  
    |VpnIPv6PrefixLength|(Opcional) Establece la longitud del prefijo de dirección IPv6 de VPN.|  
    |IsHosted|(Opcional) Valor predeterminado es "false" Si no se especifica. Establece este valor si configuras esto en el entorno de host. Se deshabilitará la configuración del enrutador.|  
    |StaticIPv4Address|(Opcional) Especificar la dirección IP estática si quieres configurar una dirección IP estática en lugar de una dirección dinámica.|  
    |StaticIPv4Gateway|(Opcional) Especificar la dirección de puerta de enlace predeterminada si quieres configurar una dirección IP estática en lugar de una dirección dinámica.|  
    |StaticIPv4SubnetMask|(Opcional) Especifica la máscara de subred si quieres configurar una dirección IP estática en lugar de una dirección dinámica.|  
    |StaticIPv6Address|(Opcional) Especificar la dirección IP predeterminada si quieres configurar una dirección IP estática en lugar de una dirección dinámica.|  
    |StaticIPv6SubnetPrefixLength|(Opcional) Especificar de forma predeterminada, la longitud de prefijo de subred IPV6 si quieres configurar una dirección IP estática en lugar de una dirección dinámica.|  
    |StaticIPv6Gateway|(Opcional) Especificar la dirección de puerta de enlace predeterminada si quieres configurar una dirección IP estática en lugar de uno dinámico.|  
    |ClientBackupOn|(Opcional) Desactivar cliente copia de seguridad predeterminada cuando el servidor de unido a nuevos clientes.|  
    |FileHistoryOn|(Opcional) Desactivar historial de archivos copia de seguridad predeterminada cuando el servidor de unido a nuevos clientes que ejecutan Windows 8 Consumer Preview.|  
    |EnableRWA|Permitirá el acceso Web remoto cuando se instala Windows Server Essentials, pero se omitirá la configuración del enrutador. Esto solo se admite en una instalación limpia del producto. El valor predeterminado es "false".|  
    |IPv4DNSForwarder|Establecer IPv4 reenviador DNS.|  
    |IPv6DNSForwarder|Establecer reenviador DNS IPv6.|  
    |LaunchPadHiddenTasks|-(Opcional) puedes ocultar la entrada de copia de seguridad o / y la entrada del panel de administración en el Launchpad.<br /><br /> -Para deshabilitar el panel: LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -Para deshabilitar la copia de seguridad: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -Para deshabilitar la copia de seguridad y el panel: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  Guarda el archivo. Asegúrate de guardar el archivo como cfg.ini, no cfg.ini.txt.  
  
    > [!NOTE]
    >  Puede guardar el archivo en una unidad flash USB, que puede usarse para las fases específicas de la instalación, o el archivo cfg.ini puede encontrarse en la raíz de cualquier unidad de disco duro en el servidor de destino. Debes asegurarte de que la codificación para el archivo se establece como ANSI o Unicode, no se admite la codificación UTF-8.  
  
> [!IMPORTANT]
>  La sección de la configuración inicial de la cfg.ini solo debe usarse el usuario final para personalizar el servidor o para un socio probar la experiencia de usuario del servidor mediante un archivo de respuesta para instalación desatendida. En esta sección del archivo no está pensada para usarse para crear la imagen.  
  
## <a name="see-also"></a>Consulta también  

 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Tareas iniciales con el ADK de Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

