---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configuración de identificador de inicio de sesión alternativo
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.openlocfilehash: 549ba062a30ce3b2d1a9f06d60357c0199766d84
ms.sourcegitcommit: c6e2e545100bbbc4864088fd0d103bafc147fcbb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2020
ms.locfileid: "88785067"
---
# <a name="configuring-alternate-login-id"></a>Configuración de identificador de inicio de sesión alternativo


## <a name="what-is-alternate-login-id"></a>¿Qué es el identificador de inicio de sesión alternativo?
En la mayoría de los escenarios, los usuarios usan sus UPN (nombres de entidad de seguridad de usuario) para iniciar sesión en sus cuentas. Sin embargo, en algunos entornos debido a las directivas corporativas o a las dependencias de la aplicación de línea de negocio local, los usuarios pueden usar algún otro tipo de inicio de sesión.

> [!NOTE]
> Los procedimientos recomendados de Microsoft hacen coincidir el UPN con la dirección SMTP principal. En este artículo se aborda el pequeño porcentaje de clientes que no pueden corregir los UPN para que coincidan.

Por ejemplo, pueden utilizar su identificador de correo electrónico para el inicio de sesión y que pueden ser diferentes de sus UPN. Esto es especialmente frecuente en escenarios en los que su UPN no es enrutable. Considere la posibilidad de que un usuario Jane Doe con el UPN jdoe@contoso.local y la dirección de correo electrónico jdoe@contoso.com . Es posible que Julia no sea aún consciente del UPN, ya que siempre ha usado su identificador de correo electrónico para iniciar sesión. El uso de cualquier otro método de inicio de sesión en lugar de UPN constituye un identificador alternativo. Para obtener más información sobre cómo se crea el UPN, consulte [Azure ad rellenado de UserPrincipalName](/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

Servicios de federación de Active Directory (AD FS) (AD FS) permite que las aplicaciones federadas usen AD FS para iniciar sesión con el identificador alternativo. Esto permite a los administradores especificar una alternativa al UPN predeterminado que se usará para el inicio de sesión. AD FS ya admite el uso de cualquier forma de identificador de usuario aceptada por Active Directory Domain Services (AD DS). Cuando se configura para el identificador alternativo, AD FS permite a los usuarios iniciar sesión con el valor de identificador alternativo configurado, por ejemplo, el identificador de correo electrónico. El uso del identificador alternativo permite adoptar proveedores de SaaS, como Office 365 sin modificar los UPN locales. También permite admitir aplicaciones de servicio de línea de negocio con identidades aprovisionadas por el consumidor.

## <a name="alternate-id-in-azure-ad"></a>Identificador alternativo en Azure AD

Una organización puede tener que usar un identificador alternativo en los escenarios siguientes:
1. El nombre de dominio local no es enrutable, por ejemplo, Contoso. local y, como resultado, el nombre principal de usuario predeterminado es no enrutable ( jdoe@contoso.local ). No se puede cambiar el UPN existente debido a las dependencias de la aplicación local o a las directivas de la empresa. Azure AD y Office 365 requieren que todos los sufijos de dominio asociados con Azure AD directorio sean totalmente enrutables a Internet.
2. El UPN local no es el mismo que la dirección de correo electrónico del usuario y, para iniciar sesión en Office 365, los usuarios usan la dirección de correo electrónico y el UPN no se puede usar debido a restricciones de la organización.
   En los escenarios mencionados anteriormente, el identificador alternativo con AD FS permite a los usuarios iniciar sesión en Azure AD sin modificar los UPN locales.

## <a name="configure-alternate-logon-id"></a>Configurar el identificador de inicio de sesión alternativo
Con Azure AD Connect se recomienda usar Azure AD Connect para configurar el identificador de inicio de sesión alternativo para su entorno.

- Para ver la nueva configuración de Azure AD Connect, consulte conexión a Azure AD para obtener instrucciones detalladas sobre cómo configurar el identificador alternativo y la granja de AD FS.
- Para las instalaciones de Azure AD Connect existentes, consulte cambio del método de inicio de sesión de usuario para obtener instrucciones sobre cómo cambiar el método de inicio de sesión a AD FS

Cuando Azure AD Connect se proporciona información detallada sobre el entorno de AD FS, comprueba automáticamente la presencia de los KB adecuados en el AD FS y configura AD FS para el ID. alternativo, incluidas todas las reglas de notificaciones correctas necesarias para Azure AD confianza de Federación. No se requiere ningún paso adicional fuera del Asistente para configurar el identificador alternativo.

> [!NOTE]
> Microsoft recomienda usar Azure AD Connect para configurar el identificador de inicio de sesión alternativo.

### <a name="manually-configure-alternate-id"></a>Configuración manual del ID. alternativo
Para configurar el identificador de inicio de sesión alternativo, debe realizar las siguientes tareas: configurar las confianzas del proveedor de notificaciones de AD FS para habilitar el identificador de inicio de sesión alternativo

1.  Si tiene el servidor 2012R2, asegúrese de que tiene KB2919355 instalado en todos los servidores AD FS. Puede obtenerla a través de Windows Update Services o descargarla directamente.

2.  Actualice la configuración de AD FS ejecutando el siguiente cmdlet de PowerShell en cualquiera de los servidores de Federación de la granja (si tiene una granja WID, debe ejecutar este comando en el servidor de AD FS principal de la granja de servidores):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** es el nombre LDAP del atributo que desea usar para el inicio de sesión.

**LookupForests** es la lista de DNS de bosque a la que pertenecen los usuarios.

Para habilitar la característica de identificador de inicio de sesión alternativo, debe configurar los parámetros-AlternateLoginID y-LookupForests con un valor válido que no sea NULL.

En el ejemplo siguiente, se habilita la funcionalidad de identificador de inicio de sesión alternativo, de modo que los usuarios con cuentas de los bosques contoso.com y fabrikam.com puedan iniciar sesión en aplicaciones habilitadas para AD FS con el atributo "mail".

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. Para deshabilitar esta característica, establezca el valor de ambos parámetros en NULL.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Autenticación moderna híbrida con ID. alternativo

> [!IMPORTANT]
> Lo siguiente solo se ha probado con AD FS y no con proveedores de identidades de terceros.

### <a name="exchange-and-skype-for-business"></a>Exchange y Skype empresarial
Si usa un identificador de inicio de sesión alternativo con Exchange y Skype empresarial, la experiencia del usuario varía en función de si está usando HMA o no.

> [!NOTE]
> Para obtener la mejor experiencia del usuario final, Microsoft recomienda usar la autenticación moderna híbrida.

o más información, consulte [Introducción a la autenticación moderna híbrida](https://support.office.com/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Requisitos previos de Exchange y Skype empresarial
A continuación se indican los requisitos previos para lograr el inicio de sesión único con el identificador alternativo.

- Exchange Online debe tener activada la autenticación moderna.
- Skype empresarial (SFB) en línea debe tener habilitada la autenticación moderna.
- Exchange local debe tener activada la autenticación moderna.  Se requiere Exchange 2013 CU19 o Exchange 2016 CU18 y up en todos los servidores de Exchange. No hay Exchange 2010 en el entorno.
- Skype empresarial local debe tener activada la autenticación moderna.
- Debe usar clientes de Skype y de Exchange que tengan habilitada la autenticación moderna. Todos los servidores deben ejecutar SFB Server 2015 CU5.
- Clientes de Skype empresarial compatibles con la autenticación moderna
   - iOS, Android, Windows Phone
   - SFB 2016 (MA está activada de forma predeterminada, pero asegúrese de que no se ha deshabilitado).
   - SFB 2013 (MA está desactivado de forma predeterminada, por lo que debe asegurarse de que se ha activado MA).
   - SFB Mac Desktop
- Clientes de Exchange que son compatibles con la autenticación moderna y admiten AltID REGKEYS
    - Office Pro Plus 2016 solo

#### <a name="supported-office-version"></a>Versión de Office compatible

La configuración del directorio para el inicio de sesión único con el identificador alternativo mediante el uso de un identificador alternativo puede producir solicitudes adicionales de autenticación si no se completan estas configuraciones adicionales. Consulte el artículo sobre el posible impacto en la experiencia del usuario con el identificador alternativo.

Con la siguiente configuración adicional, la experiencia del usuario se ha mejorado de forma significativa y se pueden conseguir solicitudes de casi cero para la autenticación de los usuarios de ID. alternativo de la organización.

##### <a name="step-1-update-to-required-office-version"></a>Paso 1. Actualizar a la versión de Office requerida
La versión 1712 de Office (compilación no 8827,2148) y versiones posteriores han actualizado la lógica de autenticación para controlar el escenario de identificador alternativo. Con el fin de aprovechar la nueva lógica, los equipos cliente deben actualizarse a la versión 1712 de Office (compilación no 8827,2148) y versiones posteriores.

##### <a name="step-2-update-to-required-windows-version"></a>Paso 2. Actualizar a la versión de Windows necesaria
La versión 1709 y posteriores de Windows han actualizado la lógica de autenticación para controlar el escenario de identificador alternativo. Con el fin de aprovechar la nueva lógica, los equipos cliente deben actualizarse a la versión 1709 y posteriores de Windows.

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>Paso 3. Configurar el registro para los usuarios afectados mediante la Directiva de grupo
Las aplicaciones de Office se basan en la información insertada por el administrador de directorios para identificar el entorno de identificador alternativo. Las siguientes claves del registro deben configurarse para ayudar a las aplicaciones de Office a autenticar al usuario con el identificador alternativo sin mostrar ningún mensaje adicional.

|RegKey que se va a agregar|Nombre, tipo y valor de datos RegKey|Windows 7/8|Windows 10|Descripción|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER \Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Obligatorio|Obligatorio|El valor de esta RegKey es un nombre de dominio personalizado comprobado en el inquilino de la organización. Por ejemplo, Contoso Corp puede proporcionar un valor de Contoso.com en esta RegKey si Contoso.com es uno de los nombres de dominio personalizados comprobados en el inquilino Contoso.onmicrosoft.com.|
HKEY_CURRENT_USER \Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Necesario para Outlook 2016 ProPlus|Necesario para Outlook 2016 ProPlus|El valor de esta RegKey puede ser 1/0 para indicar a la aplicación de Outlook si debe interactuar con la lógica de autenticación alternativa mejorada.|
HKEY_CURRENT_USER \Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obligatorio|Obligatorio|Esta clave de instalación se puede usar para establecer el STS como una zona de confianza en la configuración de Internet. La implementación de ADFS estándar recomienda agregar el espacio de nombres de ADFS a la zona de Intranet local para Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Nuevo flujo de autenticación después de la configuración adicional

![Flujo de autenticación](media/Configure-Alternate-Login-ID/alt1a.png)

1. r: el usuario se aprovisiona en Azure AD mediante el identificador alternativo
   </br>b: el administrador de directorios inserciones la configuración de RegKey necesaria en los equipos cliente afectados
2. El usuario se autentica en el equipo local y abre una aplicación de Office
3. La aplicación de Office toma las credenciales de la sesión local
4. La aplicación de Office se autentica en Azure AD mediante la sugerencia de dominio insertada por el administrador y las credenciales locales
5. Azure AD autentica correctamente al usuario mediante la dirección del dominio de Federación correcto y emite un token

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Aplicaciones y experiencia del usuario después de la configuración adicional

### <a name="non-exchange-and-skype-for-business-clients"></a>Clientes que no son de Exchange y Skype empresarial

|Cliente|Declaración de compatibilidad|Observaciones|
| ----- | -----|-----|
|Equipos de Microsoft|Compatible.|<li>Microsoft Teams admite AD FS (SAML-P, WS-FED, WS-Trust y OAuth) y la autenticación moderna.</li><li> Los principales equipos de Microsoft, como las funcionalidades de canales, chats y archivos, funcionan con un identificador de inicio de sesión alternativo.</li><li>el cliente debe investigar por separado la primera y las aplicaciones de terceros. Esto se debe a que cada aplicación tiene sus propios protocolos de autenticación.</li>|
|OneDrive para la Empresa|Compatible: se recomienda la clave del registro del lado cliente |Con el identificador alternativo configurado, verá que el UPN local se rellena previamente en el campo de comprobación. Esto debe cambiarse a la identidad alternativa que se está usando. Se recomienda usar la clave del registro del lado cliente que se indica en este artículo: Office 2013 y Lync 2013 solicitan periódicamente las credenciales a SharePoint Online, OneDrive y Lync Online.|
|Cliente móvil de OneDrive para la empresa|Compatible.||
|Página de activación de Office 365 Pro Plus|Compatible: se recomienda la clave del registro del lado cliente|Con el identificador alternativo configurado, verá que el UPN local se rellena previamente en el campo de comprobación. Esto debe cambiarse a la identidad alternativa que se está usando. Se recomienda usar la clave del registro del lado cliente que se indica en este artículo: Office 2013 y Lync 2013 solicitan periódicamente las credenciales a SharePoint Online, OneDrive y Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Clientes de Exchange y Skype empresarial

|Cliente|Declaración de soporte: con HMA|Instrucción de soporte técnico: sin HMA|
| ----- |----- | ----- |
|Outlook|Compatible, sin mensajes adicionales|Compatible.</br></br>Con **autenticación moderna** para Exchange Online: compatible</br></br>Con **autenticación normal** para Exchange Online: se admite con las siguientes advertencias:</br><li>Debe estar en un equipo unido a un dominio y estar conectado a la red corporativa </li><li>Solo se puede usar un identificador alternativo en entornos que no permiten el acceso externo a los usuarios de buzones. Esto significa que los usuarios solo pueden autenticarse en su buzón de una manera compatible cuando están conectados y Unidos a la red corporativa, en una VPN o conectados a través de máquinas de acceso directo, pero obtiene un par de mensajes adicionales al configurar el perfil de Outlook.|
|Carpetas públicas híbridas|Compatible, no hay ningún mensaje adicional.|Con **autenticación moderna** para Exchange Online: compatible</br></br>Con **autenticación normal** para Exchange Online: no compatible</br></br><li>Las carpetas públicas híbridas no pueden expandirse si se usan IDENTIFICADOres alternativos y, por lo tanto, no deben usarse actualmente con métodos de autenticación normales.|
|Delegación entre locales|Consulte [configuración de Exchange para admitir permisos de buzón delegados en una implementación híbrida](/exchange/hybrid-deployment/set-up-delegated-mailbox-permissions)|Consulte [configuración de Exchange para admitir permisos de buzón delegados en una implementación híbrida](/exchange/hybrid-deployment/set-up-delegated-mailbox-permissions)|
|Acceso al buzón de archivo (buzón local: archivar en la nube)|Compatible, sin mensajes adicionales|Compatible: los usuarios obtienen una solicitud adicional de credenciales al acceder al archivo, por lo que deben proporcionar su identificador alternativo cuando se le solicite.|
|Outlook Web Access|Compatible.|Compatible.|
|Mobile Apps de Outlook para Android, IOS y Windows Phone|Compatible.|Compatible.|
|Skype empresarial/Lync|Compatible, sin mensajes adicionales|Compatible (a menos que se indique), pero existe una posibilidad de confusión del usuario.</br></br>En los clientes móviles, el identificador alternativo solo se admite si la dirección SIP = dirección de correo electrónico = identificador alternativo.</br></br> Es posible que los usuarios tengan que iniciar sesión dos veces en el cliente de escritorio de Skype empresarial, usando primero el UPN local y, a continuación, usando el identificador alternativo. (Tenga en cuenta que la "dirección de inicio de sesión" es realmente la dirección SIP, que puede no ser la misma que el "nombre de usuario", aunque a menudo es). Cuando se solicita un nombre de usuario por primera vez, el usuario debe escribir el UPN, incluso si se ha rellenado previamente con el identificador alternativo o la dirección SIP. Después de que el usuario haga clic en el inicio de sesión con el UPN, volverá a aparecer el mensaje de nombre de usuario, esta vez rellenado con el UPN. Esta vez, el usuario debe reemplazarlo por el identificador alternativo y hacer clic en iniciar sesión para completar el proceso de inicio de sesión. En los clientes móviles, los usuarios deben escribir el ID. de usuario local en la página avanzadas, con el formato de estilo SAM (dominio\nombre de usuario), no el formato UPN.</br></br>Después del inicio de sesión correcto, si Skype empresarial o Lync dice "Exchange necesita sus credenciales", debe proporcionar las credenciales válidas para el lugar donde se encuentra el buzón. Si el buzón está en la nube, debe proporcionar el identificador alternativo. Si el buzón de correo es local, debe proporcionar el UPN local.|

## <a name="additional-details--considerations"></a>Detalles adicionales & consideraciones

- La característica de identificador de inicio de sesión alternativo está disponible para entornos federados con AD FS implementado.  No se admite en los escenarios siguientes:
    - Los dominios no enrutables (por ejemplo, contoso. local) que no se pueden comprobar mediante Azure AD.
    - Entornos administrados que no tienen AD FS implementado.


- Cuando está habilitada, la característica de identificador de inicio de sesión alternativo solo está disponible para la autenticación de nombre de usuario y contraseña en todos los protocolos de autenticación de nombre de usuario y contraseña admitidos por AD FS (SAML-P, WS-FED, WS-Trust y OAuth).


- Cuando se realiza la autenticación integrada de Windows (WIA) (por ejemplo, cuando los usuarios intentan obtener acceso a una aplicación corporativa en un equipo unido a un dominio desde la intranet y AD FS administrador ha configurado la Directiva de autenticación para usar WIA para intranet), UPN Isused para la autenticación. Si ha configurado alguna regla de notificaciones para los usuarios de confianza para la característica de identificador de inicio de sesión alternativo, debe asegurarse de que esas reglas siguen siendo válidas en el caso de WIA.

- Cuando está habilitada, la característica de identificador de inicio de sesión alternativo requiere que al menos un servidor de catálogo global sea accesible desde el servidor de AD FS para cada bosque de cuenta de usuario que AD FS admita. Si no se alcanza un servidor de catálogo global en el bosque de cuentas de usuario, AD FS se revierte al uso de UPN. De forma predeterminada, todos los controladores de dominio son servidores de catálogo global.

- Cuando está habilitada, si el servidor de AD FS encuentra más de un objeto de usuario con el mismo valor de identificador de inicio de sesión alternativo especificado en todos los bosques de cuentas de usuario configurados, se produce un error en el inicio de sesión.

- Cuando está habilitada la característica de identificador de inicio de sesión alternativo, AD FS intenta autenticar primero el usuario final con el identificador de inicio de sesión alternativo y, a continuación, revertir al uso de UPN si no encuentra una cuenta que pueda identificarse mediante el identificador de inicio de sesión alternativo. Debe asegurarse de que no haya conflictos entre el identificador de inicio de sesión alternativo y el UPN si desea seguir admitiendo el inicio de sesión UPN. Por ejemplo, si se establece el atributo de correo de un usuario con el UPN del otro, se impide que el otro usuario inicie sesión con su UPN.

- Si uno de los bosques configurados por el administrador está inactivo, AD FS continúa buscando una cuenta de usuario con un identificador de inicio de sesión alternativo en otros bosques que están configurados. Si AD FS servidor encuentra objetos de usuario únicos en los bosques en los que se ha buscado, un usuario inicia sesión correctamente.

- Además, puede que desee personalizar la página de inicio de sesión de AD FS para proporcionar a los usuarios finales alguna sugerencia sobre el ID. de inicio de sesión alternativo. Puede hacerlo agregando la descripción de la página de inicio de sesión personalizada (para obtener más información, consulte [Personalización de las páginas de inicio](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11)) de sesión de AD FS o personalización de la cadena "iniciar sesión con la cuenta de la organización" sobre el campo de nombre de usuario (para obtener más información, consulte [Personalización avanzada de páginas de inicio de sesión de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn636121(v=ws.11)).

- El nuevo tipo de notificaciones que contiene el valor del identificador de inicio de sesión alternativo es **http: schemas. Microsoft. com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Eventos y contadores de rendimiento
Se han agregado los siguientes contadores de rendimiento para medir el rendimiento de los servidores de AD FS cuando el identificador de inicio de sesión alternativo está habilitado:

- Autenticaciones de identificador de inicio de sesión alternativo: número de autenticaciones realizadas mediante el identificador de inicio de sesión alternativo

- Autenticaciones de ID. de inicio de sesión alternativo/s: número de autenticaciones realizadas mediante un identificador de inicio de sesión alternativo por segundo

- Latencia media de búsqueda para el ID. de inicio de sesión alternativo: latencia media de búsqueda en los bosques que un administrador ha configurado para el ID. de inicio de sesión alternativo

A continuación se muestran varios casos de error y el impacto correspondiente en la experiencia de inicio de sesión de un usuario con los eventos registrados por AD FS:



|                       **Casos de error**                        | **Impacto en la experiencia de inicio de sesión** |                                                              **Evento**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| No se puede obtener un valor para SAMAccountName para el objeto de usuario |          Error de inicio de sesión           |                  ID. de evento 364 con el mensaje de excepción MSIS8012: no se puede encontrar samAccountName para el usuario: ' {0} '.                   |
|        No se puede obtener acceso al atributo CanonicalName         |          Error de inicio de sesión           |               El ID. de evento 364 con el mensaje de excepción MSIS8013: CanonicalName: ' {0} ' del usuario: ' {1} ' tiene un formato incorrecto.                |
|        Se encuentran varios objetos de usuario en un bosque        |          Error de inicio de sesión           | ID. de evento 364 con el mensaje de excepción MSIS8015: se encontraron varias cuentas de usuario con la identidad ' {0} ' en el bosque ' {1} ' con identidades: {2} |
|   Se encuentran varios objetos de usuario en varios bosques    |          Error de inicio de sesión           |           ID. de evento 364 con el mensaje de excepción MSIS8014: se encontraron varias cuentas de usuario con la identidad ' {0} ' en los bosques: {1}            |

## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
