---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configuración de identificador de inicio de sesión alternativo
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 615faf4153949aa4ad989f017068d1809fca26b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820876"
---
# <a name="configuring-alternate-login-id"></a>Configuración de identificador de inicio de sesión alternativo

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

## <a name="what-is-alternate-login-id"></a>¿Qué es el identificador de inicio de sesión alternativo?
En la mayoría de los escenarios, los usuarios usar su UPN (nombres principales de usuario) para iniciar sesión en sus cuentas. Sin embargo, en algunos entornos debido a las directivas corporativas o dependencias de la aplicación de línea de negocio de forma local, los usuarios que esté utilizando algún otro método de inicio de sesión. 

>[!NOTE]
>Microsoft recomienda son los procedimientos recomendados para que coincida con el UPN a la dirección SMTP principal. Este artículo se abordan porcentaje pequeño de clientes que no se puede corregir el UPN del para hacer coincidir.

Por ejemplo, que pueden usar su identificador de correo electrónico para el inicio de sesión y que puede ser diferente de su UPN. Esto es especialmente habitual en escenarios donde su UPN no es enrutable. Considere la posibilidad de un usuario Jane Doe con UPN jdoe@contoso.local y dirección de correo electrónico jdoe@contoso.com. Julia es posible que no tenga incluso el UPN como su identificador de correo electrónico siempre ha usado para iniciar sesión. El uso de cualquier otro inicio de sesión de método en lugar del UPN constituye el identificador alternativo. Para obtener más información sobre cómo el UPN es crea, consulte [rellenado de UserPrincipalName de Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

Identificador de Active Directory Federation Services (AD FS) habilita las aplicaciones federadas mediante AD FS para iniciar sesión con alternativas. Esto permite a los administradores especificar alternativa en el valor predeterminado UPN que se usará para el inicio de sesión. AD FS ya admite el uso de cualquier forma de identificador de usuario que está aceptado por los servicios de dominio de Active Directory (AD DS). Cuando se configura para el Id. alternativo, ADFS permite a los usuarios iniciar sesión con el valor de identificador alternativo configurado, por ejemplo, identificador de correo electrónico. El identificador alternativo permite adoptar a los proveedores de SaaS, como Office 365 sin modificar su UPN local. También permite admitir aplicaciones de servicio de la línea de negocio con identidades aprovisionado por el consumidor.

## <a name="alternate-id-in-azure-ad"></a>Id. alternativo en Azure AD
Una organización que tenga que utilizar un identificador alternativo en los escenarios siguientes:
1.  El nombre de dominio local no es enrutable, p. ej. Contoso.local y como resultado el nombre principal de usuario predeterminada no es enrutable (jdoe@contoso.local). UPN existente no se puede cambiar debido a dependencias de aplicación locales o las directivas de empresa. Azure AD y Office 365 requieren todos los sufijos de dominio asociados con el directorio de Azure AD para ser totalmente enrutable de internet. 
2.  El UPN local no es igual a la dirección de correo electrónico del usuario y para iniciar sesión en Office 365, los usuarios usar la dirección de correo electrónico y UPN no se puede utilizar debido a restricciones de la organización.
En los escenarios mencionados anteriormente, Id. alternativo con AD FS permite a los usuarios iniciar sesión en Azure AD sin modificar su UPN local. 

## <a name="end-user-experience-with-alternate-login-id"></a>Experiencia del usuario final con el Id. de inicio de sesión alternativo
La experiencia del usuario final varía según el método de autenticación usado con el Id. de inicio de sesión alternativo.  Actualmente hay tres maneras diferentes en el que se puede lograr mediante el identificador de inicio de sesión alternativo.  Estas sobrecargas son:

- **Autenticación normal (heredado)**-utiliza el protocolo de autenticación básica.
- **Autenticación moderna** -aporta sesión basada en Active Directory Authentication Library ADAL a aplicaciones. Esto habilita características de inicio de sesión como Multi-factor Authentication (MFA), proveedores de identidades de terceros basado en SAML con aplicaciones cliente de Office, tarjeta inteligente y la autenticación basada en certificados.
- **Autenticación moderna híbrida** : proporciona todas las ventajas de la autenticación moderna y proporciona a los usuarios la capacidad de tener acceso a aplicaciones locales mediante los tokens de autorización obtenidos de la nube.

>[!NOTE]
> Para una mejor experiencia posible, Microsoft recomienda encarecidamente la autenticación moderna híbrida.



## <a name="configure-alternate-logon-id"></a>Configurar el identificador de inicio de sesión alternativo
Con Azure AD Connect se recomienda el uso de Azure AD connect para configurar el identificador de inicio de sesión alternativo para su entorno.

- Para la nueva configuración de Azure AD Connect, vea Conectar con Azure AD para obtener instrucciones detalladas sobre cómo configurar la granja de servidores FS AD y de identificador alternativo.
- Para las instalaciones existentes de Azure AD Connect, consulte Cambiar el método de inicio de sesión de usuario para obtener instrucciones acerca de cómo cambiar el método de inicio de sesión a AD FS

Cuando Azure AD Connect proporciona detalles sobre el entorno de AD FS, automáticamente comprueba la presencia de la KB adecuada en AD FS y configura AD FS para el Id. alternativo, incluidas todas las reglas de notificación derecho necesario para la confianza de federación de Azure AD. No hay ningún paso adicional necesario fuera para el Asistente para configurar el identificador alternativo.

>[!NOTE]
> Microsoft recomienda el uso de Azure AD Connect para configurar el identificador de inicio de sesión alternativo.

### <a name="manually-configure-alternate-id"></a>Configurar manualmente un Id. alternativo
Para configurar el identificador de inicio de sesión alternativo, debe realizar las siguientes tareas: Configurar las confianzas de proveedores de notificaciones de AD FS para habilitar el identificador de inicio de sesión alternativo

1.  Si dispone de Server 2012 R2, asegúrese de que tiene instalado en todos los servidores de AD FS de KB2919355. Puede obtenerla a través de Windows Update Services o descárguela directamente. 

2.  Actualice la configuración de AD FS ejecutando el siguiente cmdlet de PowerShell en cualquiera de los servidores de federación en la granja de servidores (si tiene una granja WID, debe ejecutar este comando en el servidor de AD FS principal en la granja de servidores):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** es el nombre LDAP del atributo que desea usar para el inicio de sesión.

**LookupForests** es la lista de DNS que los usuarios que pertenecen al bosque.

Para habilitar la característica de Id. de inicio de sesión alternativo, debe configurar los parámetros - AlternateLoginID y - LookupForests con un valor válido distinto de null.

En el ejemplo siguiente, va a habilitar la funcionalidad de Id. de inicio de sesión alternativo tal que los usuarios con cuentas en bosques contoso.com y fabrikam.com pueden iniciar sesión en aplicaciones de AD FS habilitados con su atributo "mail".

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3.  Para deshabilitar esta característica, establezca el valor para ambos parámetros es null.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Autenticación moderna híbrida con Id. alternativo

>[!IMPORTANT]
>El siguiente sólo ha probado con AD FS y no 3rd proveedores de identidades de terceros.

### <a name="exchange-and-skype-for-business"></a>Exchange y Skype para la empresa
Si usa el identificador de inicio de sesión alternativo con Exchange y Skype para la empresa, la experiencia del usuario varía en función de si usas HMA.

>[!NOTE]
>Para una mejor experiencia del usuario final, Microsoft recomienda usar la autenticación moderna híbrida.

o más información, vea [Introducción a la autenticación moderna híbrida](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Requisitos previos para Exchange y Skype para la empresa
Los siguientes son requisitos previos para lograr el SSO con el identificador alternativo.

- Exchange Online debe tener activada la autenticación moderna.
- Skype para en línea de negocio (SFB) debe tener activada la autenticación moderna.
- Exchange local debe han autenticación moderna encendido.  Exchange 2013 CU19 o Exchange 2016 CU18 y seguridad, es necesario en todos los servidores de Exchange. No hay Exchange 2010 en el entorno.
- Skype for Business en el entorno local debe han autenticación moderna encendido.
- Debe usar a los clientes de Exchange y Skype que tienen habilitada la autenticación moderna. Todos los servidores deben ejecutar SFB Server 2015 CU5.
- Skype para clientes empresariales que son compatibles con la autenticación moderna
   - iOS, Android, Windows Phone
   - SFB 2016 (MA está activado de forma predeterminada, pero asegúrese de que no se ha deshabilitado).
   - SFB 2013 (MA está desactivada de forma predeterminada, así que asegúrese de se ha activado el MA.)
   - Escritorio de Mac SFB
- Clientes de Exchange que son compatibles con la autenticación moderna y admiten claves AltID
    - Office Pro Plus 2016 solo





#### <a name="supported-office-version"></a>Versión de Office compatible

Configurar el directorio de inicio de sesión único con alternate id Using alternate-id puede provocar mensajes adicionales para la autenticación si no se completan estas configuraciones adicionales. Consulte el artículo para el posible impacto en experiencia del usuario con Id. alternativo.

Con la siguiente configuración adicional, la experiencia del usuario se ha mejorado significativamente y puede conseguir casi cero peticiones de autenticación alternativo-id a los usuarios de su organización.

##### <a name="step-1-update-to-required-office-version"></a>Paso 1. Actualizar a la versión de office necesarios
Versión de Office 1712 (no compilación ningún 8827.2148) y versiones posteriores, hemos actualizado la lógica de autenticación para controlar el escenario alternativo-id. Para poder aprovechar la nueva lógica, los equipos cliente deben actualizarse a la versión 1712 de office (no crear ningún 8827.2148) y versiones posteriores.

##### <a name="step-2-configure-registry-for-impacted-users-using-group-policy"></a>Paso 2. Configurar el registro para los usuarios afectados mediante la directiva de grupo
Las aplicaciones de office se basan en información insertada por el administrador del directorio para identificar el entorno de identificador alternativo. Las siguientes claves del registro deben configurarse para ayudar a las aplicaciones de office a autenticar al usuario con Id. alternativo sin mostrar ningún otro aviso

|Clave del registro para agregar|Valor, tipo y nombre de clave del registro de datos|Windows 7/8|Windows 10|Descripción|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Requerido|Requerido|El valor de esta clave del registro es un nombre de dominio personalizado comprobado en el inquilino de la organización. Por ejemplo, Contoso corp puede proporcionar un valor de Contoso.com en esta clave del registro si Contoso.com es uno de los nombres de dominio personalizado comprobado en el inquilino Contoso.onmicrosoft.com.|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Necesario para Outlook 2016 ProPlus|Necesario para Outlook 2016 ProPlus|El valor de esta clave del registro puede ser 1 / 0 para indicar a la aplicación de Outlook si debe interactuar con la lógica de autenticación de Id. alternativo mejorada.|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableADALatopWAMOverride</br>REG_DWORD</br>1|No disponible|Obligatorio.|Esto garantiza que Office no utiliza WAM como alt-id no es compatible con el Administrador de aplicaciones Web.|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableAADWAM</br>REG_DWORD</br>1|No disponible|Obligatorio.|Esto garantiza que Office no utiliza WAM como alt-id no es compatible con el Administrador de aplicaciones Web.|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Requerido|Requerido|Esta clave del registro puede usarse para establecer al STS como una zona de confianza en la configuración de internet. Implementación estándar de AD FS recomienda agregar el espacio de nombres ADFS para la zona Intranet Local de Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Nuevo flujo de autenticación después de la configuración adicional

![Flujo de autenticación](media/Configure-Alternate-Login-ID/alt1a.png)

1. a: Usuario está aprovisionado en Azure AD mediante el Id. alternativo
   </br>b: Administrador de directorios inserta regkey requiere configuración para equipos cliente afectados
2. Usuario se autentica en el equipo local y abre una aplicación de office
3. Aplicación de Office toma las credenciales de sesión local
4. Aplicación de Office se autentica en Azure AD mediante la sugerencia de dominio insertado por el administrador y credenciales locales
5. Azure AD autentica correctamente al usuario dirigiendo al corregir el dominio de federación y emitir un token

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Las aplicaciones y experiencias de usuario después de la configuración adicional

### <a name="non-exchange-and-skype-for-business-clients"></a>No es de Exchange y Skype para los clientes empresariales
|Remoto|Declaración de soporte técnico|Comentarios|
| ----- | -----|-----|
|Microsoft Teams|Se admite|<li>Microsoft Teams es compatible con AD FS (SAML-P, WS-Fed, WS-Trust y OAuth) y la autenticación moderna.</li><li> Core Microsoft Teams, como las funcionalidades de canales, charlas y archivos funcionan con el identificador de inicio de sesión alternativo.</li><li>las aplicaciones de terceros 1 y 3 se deben investigar por separado por el cliente. Esto es porque cada aplicación tiene sus propios protocolos de autenticación de compatibilidad.</li>|     
|OneDrive para la Empresa|Compatible; se recomienda la clave del registro del lado cliente |Con el identificador alternativo configurado verá que el UPN local se rellena previamente en el campo de comprobación. Esto se debe cambiarse a la identidad alternativa que se utiliza. Se recomienda usar la clave de registro del lado cliente se ha indicado en este artículo: Office 2013 y Lync 2013 piden periódicamente las credenciales para Lync Online, SharePoint Online y OneDrive.|
|Cliente móvil empresarial de OneDrive para|Se admite|| 
|Página de activación de Office 365 Pro Plus|Compatible; se recomienda la clave del registro del lado cliente|Con el identificador alternativo configurado verá que el UPN local se rellena previamente en el campo de comprobación. Esto se debe cambiarse a la identidad alternativa que se utiliza. Se recomienda usar la clave del registro del lado cliente se ha indicado en este artículo: Office 2013 y Lync 2013 piden periódicamente las credenciales para Lync Online, SharePoint Online y OneDrive.|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange y Skype para los clientes empresariales

|Remoto|Declaración de soporte técnico: con HMA|Declaración de soporte técnico: sin HMA|
| ----- |----- | ----- |
|Outlook|Indicaciones adicionales no y compatibles|Se admite</br></br>Con **autenticación moderna** para Exchange Online: Se admite</br></br>Con **autenticación normal** para Exchange Online: Compatible con las siguientes observaciones:</br><li>Debe estar en una máquina unida a dominio y conectado a la red corporativa </li><li>Solo puede usar un identificador alternativo en entornos que no permiten el acceso externo para los usuarios de buzón. Esto significa que los usuarios pueden solo autenticarse en su buzón de manera compatible cuando conectados y unirse a la red corporativa, en una VPN o conectados a través de las máquinas de acceso directo, pero obtendrá un par de mensajes adicionales al configurar su perfil de Outlook.| 
|Carpetas públicas de híbrido|Indicaciones adicionales no y compatibles.|Con **autenticación moderna** para Exchange Online: Se admite</br></br>Con **autenticación normal** para Exchange Online: No se admite</br></br><li>Carpetas públicas híbrida no es posible expandir si los identificadores alternativos se usan y, por tanto, no debe usarse hoy mismo con los métodos de autenticación normal.|
|Entre locales de delegación|Consulte [configurar Exchange para admitir permisos de buzón delegados en una implementación híbrida](https://technet.microsoft.com/library/mt784505.aspx)|Consulte [configurar Exchange para admitir permisos de buzón delegados en una implementación híbrida](https://technet.microsoft.com/library/mt784505.aspx)|
|Acceso al buzón de archivo (buzón local - archive en la nube)|Indicaciones adicionales no y compatibles|Compatibles, los usuarios obtienen un símbolo del sistema adicional para las credenciales al acceder al archivo, debe proporcionar su Id. alternativo cuando se le solicite.| 
|Outlook Web Access|Se admite|Se admite|
|Aplicaciones móviles de Outlook para Android, IOS y Windows Phone|Se admite|Se admite|
|Skype for Business o Lync|Admite, sin solicitudes adicionales|Compatible (excepto como se indica) pero no hay posibilidad de confusión de los usuarios.</br></br>En los clientes móviles, Id. alternativo solo se admite si dirección SIP = dirección de correo electrónico = identificador alternativo.</br></br> Los usuarios que deba iniciar sesión en dos veces el Skype para cliente de escritorio de negocios, primero mediante el UPN local y, a continuación, utilizando el identificador alternativo. (Tenga en cuenta que "inicio de sesión de la dirección" es realmente la dirección SIP que no puede ser el mismo que el "nombre de usuario", aunque a menudo es). Cuando primero se le pida un nombre de usuario, el usuario debe escribir el UPN, incluso si incorrectamente se rellena previamente con la dirección SIP o de identificador alternativo. Después de que el usuario hace clic en iniciar sesión con el UPN, el usuario vuelve a aparecer el símbolo del sistema de nombre, esta vez rellena previamente con el UPN. Esta vez el usuario debe reemplazar por el identificador alternativo y haga clic en iniciar sesión para completar el proceso de inicio de sesión. En los clientes móviles, los usuarios deben escribir el identificador de usuario local en la página Opciones avanzadas, con formato de estilo de SAM (DOMINIO\nombre de usuario), formato UPN no.</br></br>Tras el inicio de sesión correcto, si dice de Skype para la empresa o Lync "Exchange necesita sus credenciales", deberá proporcionar las credenciales que son válidas para que se encuentra el buzón. Si el buzón está en la nube que debe proporcionar el identificador alternativo. Si el buzón está en el entorno local que deba proporcionar el UPN local.| 
 
## <a name="additional-details--considerations"></a>Consideraciones de & detalles adicionales

-   La característica de Id. de inicio de sesión alternativo está disponible para entornos federados con AD FS implementado.  No se admite en los escenarios siguientes:
    -   No se pueden enrutar dominios (por ejemplo, Contoso.local) que no se puede comprobar mediante Azure AD.
    -   Administrar entornos que no tiene AD FS implementado.


-   Cuando se habilita, la característica de Id. de inicio de sesión alternativo solo está disponible para la autenticación de usuario y contraseña en todos los protocolos de autenticación de nombre y contraseña de usuario que se compatibles con AD FS (SAML-P, WS-Fed, WS-Trust y OAuth).


-   Cuando se realiza la autenticación integrada de Windows (WIA) (por ejemplo, cuando los usuarios intentan tener acceso a una aplicación en un equipo unido al dominio corporativa de intranet y Administrador de AD FS ha configurado la directiva de autenticación para usar WIA para intranet), Seutiliza UPN para la autenticación. Si ha configurado ninguna regla de notificación para los usuarios de confianza para la característica de Id. de inicio de sesión alternativo, debe asegurarse de que esas reglas sigan siendo válidas en el caso de WIA.

-   Cuando se habilita, la característica de Id. de inicio de sesión alternativo requiere al menos un servidor de catálogo global para ser accesibles desde el servidor de AD FS para cada bosque de cuenta de usuario que es compatible con AD FS. Da como resultado un error al comunicarse con un servidor de catálogo global en el bosque de cuentas de usuario en AD FS recurrir a UPN. De forma predeterminada, todos los controladores de dominio son servidores de catálogo global.

-   Cuando se habilita, si el servidor de AD FS encuentra más de un objeto de usuario con el mismo valor de Id. de inicio de sesión alternativo especificado entre todos los bosques de cuenta de usuario configurado, produce un error en el inicio de sesión.

-   Cuando está habilitada la característica de Id. de inicio de sesión alternativo, AD FS intenta autenticar el usuario final con el Id. de inicio de sesión alternativo en primer lugar y, a continuación, revertir para utilizar el UPN si no encuentra una cuenta que puede identificarse por el identificador de inicio de sesión alternativo. Debe asegurarse de que no hay ningún entra en conflicto entre el identificador de inicio de sesión alternativo y el UPN si desea admitir el inicio de sesión UPN. Por ejemplo, establecer el atributo de correo electrónico de uno con las demás UPN bloquea el otro usuario de inicio de sesión con su UPN.

-   Si uno de los bosques que se configura el administrador está inactivo, AD FS continúa buscar la cuenta de usuario con el Id. de inicio de sesión alternativo en otros bosques que están configurados. Si el servidor de AD FS encuentra a un único usuario objetos entre los bosques que ha buscado en, un usuario inicia sesión correctamente.

-   Además es posible que desee personalizar la página de inicio de sesión de AD FS para dar a los usuarios finales alguna sugerencia sobre el identificador de inicio de sesión alternativo. Puede hacerlo agregando la descripción de la página de inicio de sesión personalizada (para obtener más información, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) o personalizar la cadena "Iniciar sesión con cuenta profesional" por encima del campo de nombre de usuario (para obtener más información obtener información, consulte [personalización avanzada de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).

-   El nuevo tipo de notificación que contiene el valor de Id. de inicio de sesión alternativo es **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Eventos y contadores de rendimiento
Se han agregado los siguientes contadores de rendimiento para medir el rendimiento de servidores de AD FS cuando se habilita el identificador de inicio de sesión alternativo:

-   Autenticaciones de Id. de inicio de sesión alternativas: número de autenticaciones que se realizan mediante el uso de Id. de inicio de sesión alternativo

-   Alternar las autenticaciones de Id. de inicio de sesión/seg.: número de autenticaciones que se realizan mediante el uso de Id. de inicio de sesión alternativo por segundo

-   Promedio de latencia de búsqueda para el Id. de inicio de sesión alternativo: promedio de latencia de búsqueda entre los bosques que un administrador ha configurado para el Id. de inicio de sesión alternativo

Estos son los distintos casos de error y su repercusión en la experiencia del usuario de inicio de sesión con los eventos registrados por AD FS:



**Casos de error**|**Impacto en la experiencia de inicio de sesión**|**Evento**|
---------|---------|---------
No se puede obtener un valor para el atributo SAMAccountName para el objeto de usuario|Error de inicio de sesión|Id. de evento 364 con mensaje de excepción MSIS8012: No se encuentra el atributo samAccountName para el usuario: '{0}'.|
El atributo CanonicalName no es accesible|Error de inicio de sesión|Id. de evento 364 con mensaje de excepción MSIS8013: CanonicalName: '{0}' del usuario:'{1}' está en un formato incorrecto.|
Varios objetos de usuario se encuentran en bosques|Error de inicio de sesión|Id. de evento 364 con mensaje de excepción MSIS8015: Se encontró varias cuentas de usuario con la identidad '{0}'en el bosque'{1}' con identidades: {2}|
Se encontraron varios objetos de usuario en varios bosques|Error de inicio de sesión|Id. de evento 364 con mensaje de excepción MSIS8014: Se encontró varias cuentas de usuario con la identidad '{0}' en bosques: {1}|

## <a name="see-also"></a>Vea también
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)


