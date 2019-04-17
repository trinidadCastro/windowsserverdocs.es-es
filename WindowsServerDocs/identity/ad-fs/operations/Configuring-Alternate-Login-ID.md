---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: "Configurar un identificador de inicio de sesión alternativo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb4c98ff1090a9d3c35654614a43e12db99d691d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-alternate-login-id"></a>Configurar un identificador de inicio de sesión alternativo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Los usuarios pueden iniciar sesión con las aplicaciones de servicios de federación de Active Directory (AD FS) habilitado mediante cualquier forma de identificador de usuario que acepte los servicios de dominio de Active Directory (AD DS). Estos incluyen nombres Principal de usuario (UPN) (johndoe@contoso.com) o de dominio completo nombres de cuenta sam (contoso\johndoe o contoso.com\johndoe #).

En determinados entornos, debido a la directiva corporativa o dependencias de aplicaciones de línea de negocio de local, los usuarios finales pueden solo tenga su nombre de dirección y no a sus cuentas sam o UPN de correo electrónico. En algunos casos, también es no se pueden enrutar el UPN (jdoe@contoso.local) y solo se usa para autenticar en aplicaciones de la red corporativa.

Dado que no se pueden enrutar dominios (ex. No se puede comprobar la propiedad contoso.local), Office 365 requiere el inicio de sesión de usuario de todos los identificadores para ser totalmente internet pueden enrutar. Si el UPN local usa un dominio no se pueden enrutar (ex. Contoso.local), o el UPN existente no se puede cambiar debido a las dependencias de la aplicación local, te recomendamos configurar sesión alternativo. Id. de inicio de sesión alternativo te permite configurar un registro de experiencia donde los usuarios pueden iniciar sesión con un atributo que no sea su UPN, como el correo.

Una de las ventajas de esta característica es que permite adoptar los proveedores de SaaS, como Office 365 sin modificar los UPN local. También permite admiten aplicaciones de servicio de la línea de negocio con identidades aprovisionados consumidor.

> [!IMPORTANT]
> Usando el identificador alternativa en entornos híbridos con Exchange o Skype para empresas es compatible pero no se recomienda. Usar el mismo conjunto de credenciales (por ejemplo, el UPN) local y en línea, proporciona la mejor experiencia de usuario en un entorno híbrido.  Microsoft recomienda a los clientes cambiar sus UPN si es posible para evitar la necesidad de que el identificador alternativo. Con ID alternativo con Lync o Skype empresarial requiere Lync Server 2013 o posterior. Deben tener en cuenta los clientes que usan el identificador alternativo habilitar [autenticación modernos](https://support.office.com/article/using-office-365-modern-authentication-with-office-clients-776c0036-66fd-41cb-8928-5495c0f9168a) Exchange en Office 365 para una mejor experiencia de usuario. Además, los clientes que usan Skype para empresas con los clientes móviles deben asegurarse de que la dirección SIP es idéntica a la dirección de correo del usuario (e identificador alternativo). 

Consulte la siguiente tabla para la experiencia del usuario con el identificador alternativo con varios clientes de Office 365 con autenticación normal, autenticación moderno y certificado de autenticación (requiere la habilitación de autenticación modernos).

|**Tipos de cliente**|**Información adicional**|**Admite instrucción - autenticación normal y moderna**|**Descripción**|
|--------------------|------------------------------|------------------------------|------------------------------|
|Outlook|Autenticación normal: Deben estar en una máquina de estén unidos a un dominio y conectado a la red corporativa<br /><br />Autenticación moderna: compatibles|Solo puedes usar identificador alternativa en entornos que no permiten el acceso externo para los usuarios de buzón. Esto significa que los usuarios puedan solo autenticarse buzón allí de forma compatible cuando están conectados y Unidos a la red corporativa, a una VPN o conectados a través de acceso directo. Si eliges para configurar la autenticación modernos (conocida como ADAL) puede utilizar Outlook desde equipos que no del dominio unido y conectado, pero obtendrá un par de preguntas adicionales al configurar el perfil de Outlook.<br /><br />Consulta la primera imagen debajo de la tabla de demostración de la experiencia de usuario.|[Autenticación moderna de Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Carpetas públicas híbrido|Autenticación normal: No admite la<br /><br />Autenticación moderna: compatibles|Las carpetas públicas híbrida no podrán expande si alternativo identificadores se usan y, por tanto, no debe usarse hoy con métodos de autenticación normal. Si quieres poder usar la carpeta pública de híbrida tendrás que configurar la autenticación modernos (conocida como ADAL).<br /><br />Consulta la primera imagen debajo de la tabla de demostración de la experiencia de usuario.|[Autenticación moderna de Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Cruzar local la delegación|No es compatible|Actualmente entre instalaciones permisos no son compatibles con una configuración híbrida, pero también no funcionarán si usas AltID.||
|Acceso al buzón de archivo (buzón-local - archivo en la nube)|Admite|Los usuarios recibirán un mensaje adicional en las credenciales al acceder al archivo, deberá proporcionar hay alternativas identificador cuando se te solicite.<br /><br />Consulta la primera imagen debajo de la tabla de demostración de la experiencia de usuario.||
|Página de activación de Office 365 Pro Plus|Admite - recomendado de la clave de registro del lado cliente|Con el identificador alternativo configurado, verás que el UPN local se rellena previamente en el campo de verificación. Esto debe cambiarse a la identidad alternativa que se está usando. Te recomendamos que uses la clave del registro de lado cliente se explicó en la columna vínculo.<br /><br />Consulta la segunda imagen debajo de la tabla de demostración de la experiencia de usuario.|[Office 2013 y Lync 2013 periódicamente pedir credenciales en SharePoint Online, OneDrive y Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|Equipos de Microsoft|Admite|Microsoft Teams es compatible con AD FS (SAML P, WS-Federal de tecnología, WS-Trust y OAuth) y autenticación modernos.<br/><br/> Core Teams de Microsoft, como las funcionalidades de canales, chats y archivos de trabajar con Altnernate Id. </br></br>Aplicaciones de la parte 1 y 3 tienen investigarse por separado por el cliente. Esto es porque cada aplicación tiene sus propia protocolos de autenticación de compatibilidad.| 
|Skype empresarial o Lync|Admite (excepto en lo que se indique lo contrario) pero no hay una posibilidad de confundir al usuario.  En los clientes móviles, se admite Id alternativo solo si dirección SIP = dirección de correo electrónico = ID alternativo.|Los usuarios pueden necesitar iniciar sesión en dos veces el Skype para el cliente de escritorio de empresas, primero con el UPN local y, a continuación, con el identificador de alternativas. (Ten en cuenta que "inicio de sesión en la dirección" es realmente la dirección SIP que no puede ser el mismo que el "nombre de usuario", aunque a menudo es).  Cuando primero se te pida un nombre de usuario, el usuario debe escribir el UPN, incluso si incorrectamente se rellena previamente con la dirección alternativa ID o SIP. Después de que el usuario hace clic en iniciar sesión con el UPN, el usuario volverá a aparecer el símbolo del sistema de nombre, esta vez previamente con el UPN. Esta vez el usuario debe reemplazar por el identificador alternativos y haz clic en iniciar sesión en completar el proceso de sesión. En clientes móviles, los usuarios deben especificar el identificador de usuario local en la página avanzada, con formato de estilo de SAM (DOMINIO\nombre de usuario), no el formato UPN.<br /><br />Después de sesión correcto, si Skype empresarial o Lync dice "Exchange necesita sus credenciales", debes proporcionar las credenciales que son válidas para donde se encuentra el buzón. Si el buzón está en la nube que tengas que proporcionar el identificador alternativo. Si el buzón está local que tengas que proporcionar el UPN local.|[Autenticación moderna de Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Outlook Web Access|Admite|||
|Aplicaciones móviles de Outlook para Android, IOS y Windows Phone|Admite|||
|OneDrive para la empresa|Admite - recomendado de la clave de registro del lado cliente|Con el identificador alternativo configurado, verás que el UPN local se rellena previamente en el campo de verificación. Esto debe cambiarse a la identidad alternativa que se está usando. Te recomendamos que uses la clave del registro de lado cliente se explicó en la columna vínculo.<br /><br />Consulta la segunda imagen debajo de la tabla de demostración de la experiencia de usuario.|[Office 2013 y Lync 2013 periódicamente pedir credenciales en SharePoint Online, OneDrive y Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|OneDrive para los clientes móviles de empresa|Admite|||

![Inicio de sesión alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID1.png)

![Inicio de sesión alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID2.png)

![Inicio de sesión alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID3.png)

A continuación, las siguientes capturas de pantalla son un ejemplo adicional con Skype para empresas.  En el ejemplo se usa la siguiente información


- SIP:userA@contoso.com 
- UPN:userA@contoso.local
- Correo electrónico:userA@contoso.com
- AltId:userA@contoso.com 

Escribe la dirección SIP en el campo de inicio de sesión.

![Skype](media/Configure-Alternate-Login-ID/SkypeA.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeB.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeC.png)

## <a name="to-configure-alternate-login-id"></a>Para configurar el Id. de inicio de sesión alternativo
Para configurar el Id. de inicio de sesión alternativo, debes realizar las siguientes tareas:

Configurar tus confianzas de proveedor de notificaciones de AD FS para habilitar el Id. de inicio de sesión alternativo

1.  Instalar [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590).  Puedes acceder a ella de Windows Update Services o descargarla directamente.

2.  Actualizar la configuración de AD FS ejecutando el siguiente cmdlet de PowerShell en cualquiera de los servidores de federación de la batería (si tienes una granja de servidores WID, debes ejecutar este comando en el servidor de AD FS principal en el conjunto de):

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>

    ```

    **AlternateLoginID** es el nombre LDAP del atributo que quieras usar para iniciar sesión.

    **LookupForests** es la lista de bosque DNS que pertenecen a los usuarios.

    Para habilitar la característica de Id. de inicio de sesión alternativo, debes configurar parámetros - AlternateLoginID y - LookupForests con un valor no null, válido.

    En el ejemplo siguiente, se habilita funcionalidad de Id. de alternativas de inicio de sesión que los usuarios con cuentas en bosques contoso.com y fabrikam.com pueden iniciar sesión en las aplicaciones de AD FS habilitado con su atributo "correo".

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
    ```

3.  Para deshabilitar esta característica, Establece el valor para ambos parámetros será null.

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
    ```

4.  Para habilitar el Id. de inicio de sesión alternativo con Azure AD, no hay pasos de configuraciones adicionales son necesarios cuando con Azure AD Connect.   Id. de alternativa puede configurarse directamente desde el asistente.  Consulta identifica los usuarios en la sección [conectar a Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/#connect-to-azure-ad).

## <a name="additional-details--considerations"></a>Consideraciones y detalles adicionales

-   La característica de identificador de inicio de sesión alternativo solo está disponible para entornos federados con AD FS implementado.  No se admite en los siguientes escenarios:
    -   No se pueden enrutar dominios (por ejemplo, Contoso.local) que no se puede comprobar por Azure AD.
    -   Administra los entornos que no tienen AD FS implementado.


-   Cuando está habilitada, la característica de identificador de inicio de sesión alternativo solo está disponible para la autenticación de nombre de usuario y contraseña en todos los protocolos de autenticación de nombre y contraseña de usuario que se compatible con AD FS (SAML P, WS-Federal de tecnología, WS-Trust y OAuth).


-   Cuando se realiza la autenticación integrada de Windows (WIA) (por ejemplo, cuando los usuarios intentan acceder a una aplicación en un equipo unido al dominio corporativa de intranet y Administrador de AD FS ha configurado la directiva de autenticación para usar WIA para intranet), se usará el UPN para la autenticación. Si has configurado las reglas de notificación para los usuarios de confianza de característica de Id. de inicio de sesión alternativo, asegúrate de que esas reglas continúan siendo válidas en el caso de WIA.

-   Cuando está habilitada, la característica de identificador de inicio de sesión alternativo requiere al menos un servidor de catálogo global para ser accesibles desde el servidor de AD FS para cada bosque de cuenta de usuario que es compatible con AD FS. No se puede alcanzar el servidor de catálogo global en el bosque de la cuenta de usuario producirá recuperaran para usar el UPN de AD FS. De manera predeterminada, todos los controladores de dominio son servidores de catálogo global.

-   Cuando se habilita, si el servidor de AD FS busca más de un objeto de usuario con el mismo valor de Id. de alternativas de inicio de sesión especificado a través de los bosques de cuenta de usuario configurada, se producirá el inicio de sesión.

-   Cuando se habilita la característica de Id. de inicio de sesión alternativo, AD FS intentará autenticar al usuario final con el identificador de inicio de sesión alternativo primero y después volver para usar UPN si no encuentra una cuenta que se puede identificar mediante el identificador de inicio de sesión alternativo. Asegúrate de que no hay ningún conflictos entre el identificador de alternativas de inicio de sesión y el UPN si quieres admitir el inicio de sesión UPN. Por ejemplo, el atributo de configuración del correo con el UPN de la otra bloqueará el otro usuario de inicio de sesión con su UPN.

-   Si uno de los bosques que se configura mediante el administrador no funciona, AD FS continuará buscar la cuenta de usuario con el identificador de alternativas de inicio de sesión en otros bosques que están configurados. Si el servidor de AD FS busca objetos de un único usuario bosques ha buscado, un usuario iniciará la sesión correctamente.

-   Además puedes personalizar la página de inicio de sesión de AD FS para dar a los usuarios finales algunos sugerencia sobre el identificador de inicio de sesión alternativo. Puedes hacerlo agregando la descripción de la página de inicio de sesión personalizada (para obtener más información, consulta [personalización de las páginas de signo en AD FS](https://technet.microsoft.com/library/dn280950.aspx) o la personalización de la cadena "que inicia sesión con cuenta corporativa" por encima del campo de nombre de usuario (para obtener más información, consulta [personalización avanzada de AD FS Sign-in páginas](https://technet.microsoft.com/library/dn636121.aspx).

-   El nuevo tipo de notificación que contiene el valor de identificador de inicio de sesión alternativa es **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Contadores de rendimiento y eventos
Se agregaron los siguientes contadores de rendimiento para medir el rendimiento de los servidores de AD FS cuando está habilitado el Id. de inicio de sesión alternativas:

-   Autenticaciones alternativos de Id. de inicio de sesión: número de autenticaciones realizadas a través de Id. de inicio de sesión alternativo

-   Alternativas Id. de inicio de sesión autenticaciones por segundo: número de autenticaciones realizadas a través de Id. de inicio de sesión alternativo por segundo

-   Promedio de latencia de búsqueda para alternativo ID: promedio de latencia de búsqueda a través de los bosques que un administrador ha configurado para ID alternativo

Estos son los distintos casos de error y correspondiente impacto en la experiencia del usuario de inicio de sesión con eventos registrados por AD FS:



**Casos de error**|**Impacto en la experiencia de inicio de sesión**|**Evento**|
---------|---------|---------
No se puede obtener un valor SAMAccountName del objeto de usuario|Error al iniciar sesión|Identificador de evento 364 con el mensaje de excepción MSIS8012: no puedes encontrar samAccountName para el usuario: '{0}'.|
El atributo CanonicalName no es accesible|Error al iniciar sesión|Identificador de evento 364 con el mensaje de excepción MSIS8013: CanonicalName: '{0}' del usuario: '{1}' tiene un formato incorrecto.|
Varios objetos de usuario se encuentran en una sola bosques|Error al iniciar sesión|Identificador de evento 364 con el mensaje de excepción MSIS8015: encontrar varias cuentas de usuario con la identidad de '{0}' en el bosque '{1}' con identidades: {2}|
Varios objetos de usuario se encuentran en varios bosques|Error al iniciar sesión|Identificador de evento 364 con el mensaje de excepción MSIS8014: encontrar varias cuentas de usuario con la identidad de '{0}' en bosques: {1}|

## <a name="see-also"></a>Consulta también
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)


