---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurar la compatibilidad de AD FS para la autenticación de certificados de usuario
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4aa2f219852dc97833365645e7455f8141a0988e
ms.sourcegitcommit: d23f880e144acf0912831557c70f777d48e3152b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632789"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configuración de AD FS para la autenticación de certificados de usuario

La autenticación de certificados de usuario se usa principalmente en dos casos de uso
* Los usuarios utilizan tarjetas inteligentes para iniciar sesión en su sistema AD FS
* Los usuarios usan certificados aprovisionados para dispositivos móviles


## <a name="prerequisites"></a>Requisitos previos
1) Determine el modo de AD FS autenticación de certificado de usuario que desea habilitar mediante uno de los modos descritos en [este artículo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) .
2) Asegúrese de que la cadena de confianza de certificados de usuario está instalada & de confianza para todos los servidores AD FS y WAP, incluidas las entidades de certificación intermedias. Normalmente, esto se hace a través de un GPO en servidores AD FS/WAP.
3)  Asegúrese de que el certificado raíz de la cadena de confianza para los certificados de usuario se encuentra en el almacén NTAuth en Active Directory
4) Si usa AD FS en el modo de autenticación de certificados alternativo, asegúrese de que los servidores AD FS y WAP tengan certificados SSL que contengan el nombre de host AD FS con el prefijo "certauth", por ejemplo "certauth.fs.contoso.com", y que se permita el tráfico a este nombre de host a través del firewall.
5) Si usa la autenticación de certificado de la extranet, asegúrese de que se puede acceder desde Internet al menos a un AIA y al menos a una ubicación de CDP o OCSP de la lista especificada en los certificados.
6) Además, para la autenticación de certificados de Azure AD, para los clientes de Exchange ActiveSync, el certificado de cliente debe tener la dirección de correo electrónico enrutable de los usuarios en Exchange online en el nombre de la entidad de seguridad o el valor de nombre de RFC822 del campo Nombre alternativo del sujeto. (Azure Active Directory asigna el valor de RFC822 al atributo de dirección del proxy en el directorio).
7) Al utilizar una autenticación basada en certificados o tarjeta inteligente, el sujeto del certificado podría no coincidir con el UserPricipalName de la cuenta de AD. En este caso, se produce un error de inicio de sesión con "usuario no encontrado".


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurar AD FS para la autenticación de certificado de usuario  

Habilite la autenticación de certificados de usuario como método de autenticación de intranet o extranet en AD FS, mediante la consola de administración de AD FS o el cmdlet de PowerShell `Set-AdfsGlobalAuthenticationPolicy` .

Si está configurando AD FS para la autenticación de certificados de Azure AD, asegúrese de que ha configurado la [Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) y las [reglas de notificaciones de AD FS necesarias](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) para el emisor y el número de serie del certificado.

Además, hay algunos aspectos opcionales.
- Si desea usar notificaciones basadas en campos y extensiones de certificado además de EKU (tipo de notificación https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku) , configure las reglas de paso de notificación adicionales en la Active Directory confianza del proveedor de notificaciones.  A continuación encontrará una lista completa de las notificaciones de certificado disponibles.  
- Si necesita restringir el acceso en función del tipo de certificado, puede usar las propiedades adicionales en el certificado en AD FS reglas de autorización de emisión para la aplicación. Los escenarios comunes son "permitir solo certificados aprovisionados por un proveedor de MDM" o "permitir solo certificados de tarjeta inteligente"
>[!IMPORTANT]
> Los clientes que usan el flujo de código de dispositivo para la autenticación y la autenticación de dispositivos con un IDP distinto de Azure AD (por ejemplo, AD FS) no podrán aplicar el acceso basado en dispositivos (por ejemplo, permitir solo los dispositivos administrados que usan un servicio MDM de terceros) para Azure AD recursos. Para proteger el acceso a los recursos corporativos en Azure AD y evitar la fuga de datos, los clientes deben configurar Azure AD el acceso condicional basado en el dispositivo (es decir, "requerir que el dispositivo se marque como queja" en Azure AD acceso condicional).
- Configure las entidades de certificación de emisión permitidas para los certificados de cliente siguiendo las instrucciones de la sección "administración de emisores de confianza para la autenticación de cliente" en [este artículo](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).
- Puede que desee considerar la posibilidad de modificar las páginas de inicio de sesión para que se adapten a las necesidades de los usuarios finales al realizar la autenticación de certificados. Los casos comunes son para (a) cambiar el "Inicio de sesión con el certificado X509" por algo más descriptivo para el usuario final

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurar la autenticación de certificados sin problemas para el explorador Chrome en escritorios de Windows
Cuando hay varios certificados de usuario (como certificados de Wi-Fi) en el equipo que cumplen los objetivos de la autenticación del cliente, el explorador Chrome del escritorio de Windows le pedirá al usuario que seleccione el certificado adecuado. Esto puede resultar confuso para el usuario final. Para optimizar esta experiencia, puede establecer una directiva para que Chrome seleccione automáticamente el certificado adecuado para mejorar la experiencia del usuario. Esta Directiva se puede establecer manualmente si se realiza un cambio en el registro o se configura automáticamente a través de GPO (para establecer las claves del registro). Esto requiere que los certificados de cliente de usuario para la autenticación en AD FS tengan emisores distintos de otros casos de uso. 

Para obtener más información sobre cómo configurar esto para Chrome, consulte este [vínculo](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshoot-certificate-authentication"></a>Solucionar problemas de autenticación de certificados
Este documento se centra en problemas comunes de solución de problemas cuando AD FS está configurado para la autenticación de certificados para los usuarios. 

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>Comprobar si los emisores de confianza de certificados están configurados correctamente en todos los servidores AD FS/WAP
*Síntoma común: HTTP 204 "sin contenido de HTTPS \: //certauth.ADFS.contoso.com"*

AD FS utiliza el sistema operativo Windows subyacente para demostrar la posesión del certificado de usuario y asegurarse de que coincide con un emisor de confianza mediante la validación de la cadena de confianza de certificados. Para que coincida con el emisor de confianza, debe asegurarse de que todas las autoridades raíz e intermedias estén configuradas como emisores de confianza en el almacén de entidades de certificación del equipo local. Para validar esto automáticamente, use la [herramienta analizador de diagnóstico de AD FS](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze). La herramienta consulta todos los servidores y garantiza que los certificados correctos se hayan aprovisionado correctamente. 
1)  Descargue y ejecute la herramienta según las instrucciones proporcionadas en el vínculo anterior.
2)  Cargar los resultados y revisar los errores

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Compruebe si la autenticación de certificados está habilitada en la Directiva de autenticación de AD FS
AD FS no habilita la autenticación de certificados de forma predeterminada. Consulte el principio de este documento sobre cómo habilitar la autenticación de certificados. 

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Compruebe si la autenticación de certificados está habilitada en la Directiva de autenticación de AD FS
AD FS realiza la autenticación de certificados de usuario de forma predeterminada en el puerto 49443 con el mismo nombre de host que AD FS (por ejemplo, `adfs.contoso.com` ). También puede configurar AD FS para usar el puerto 443 (Puerto HTTPS predeterminado) mediante el enlace SSL alternativo. Sin embargo, la dirección URL usada en esta configuración es `certauth.<adfs-farm-name>` (por ejemplo, `certauth.contoso.com` ). Consulte [este vínculo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) para obtener más información. El caso más común de conectividad de red es que un firewall se ha configurado incorrectamente y bloquea o interfiere el tráfico de autenticación de certificados de usuario. Normalmente, verá una pantalla en blanco o un error de servidor 500 cuando se produzca este problema. 
1)  Anote el nombre de host y el puerto que ha configurado en AD FS
2)  Asegúrese de que los firewalls delante de AD FS o el proxy de aplicación web (WAP) estén configurados para permitir la `hostname:port` combinación de la granja de AD FS. Tendrá que consultar al ingeniero de red para realizar este paso. 

### <a name="check-certificate-revocation-list-connectivity"></a>Comprobar la conectividad de la lista de revocación de certificados
Las listas de revocación de certificados (CRL) son extremos que se codifican en el certificado de usuario para realizar comprobaciones de revocación en tiempo de ejecución. Por ejemplo, si se roba un dispositivo que contenía un certificado, un administrador puede Agregar el certificado a la lista de certificados revocados. Los puntos de conexión que aceptaron este certificado anteriormente no funcionarán correctamente.

Cada AD FS y el servidor WAP deberán alcanzar el punto de conexión de CRL para validar si el certificado que se presentó todavía es válido y no se ha revocado. La validación de CRL puede producirse a través de HTTPS, HTTP, LDAP o mediante OCSP (Protocolo de estado de certificados en línea). Si los servidores AD FS/WAP no pueden alcanzar el punto de conexión, se producirá un error en la autenticación. Siga los pasos que se indican a continuación para solucionar el problema. 
1) Póngase en contacto con su ingeniero de PKI para determinar los puntos de conexión de CRL usados para revocar los certificados de usuario del sistema PKI. 
2)  En cada servidor AD FS/WAP, asegúrese de que los extremos de CRL son accesibles a través del protocolo usado (normalmente HTTPS o HTTP).
3)  Para la validación avanzada, [habilite el registro de eventos CAPI2](https://blogs.msdn.microsoft.com/benjaminperkins/2013/09/30/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues/) en cada servidor AD FS/WAP.
4) Busque el ID. de evento 41 (comprobar la revocación) en los registros operativos de CAPI2
5) Comprobar`‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>'`

***Sugerencia***: puede tener como destino un solo servidor AD FS o WAP para facilitar la solución de problemas mediante la configuración de la resolución de DNS (archivo de host en Windows) para que apunte a un servidor específico. Esto le permite habilitar el seguimiento dirigido a un servidor. 

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>Compruebe si se trata de un problema de Indicación de nombre de servidor (SNI)
AD FS requiere que el dispositivo cliente (o los exploradores) y los equilibradores de carga admitan SNI. Es posible que algunos dispositivos cliente (normalmente versiones anteriores de Android) no admitan SNI. Además, es posible que los equilibradores de carga no admitan SNI o no estén configurados para SNI. En estos casos, es probable que vea errores de certificación de usuario. 
1)  Trabaje con su ingeniero de red para asegurarse de que la Load Balancer para AD FS/WAP admita SNI
2)  En caso de que no se admita SNI AD FS tiene una solución alternativa mediante los pasos siguientes:
    *   Abra una ventana de símbolo del sistema con privilegios elevados en el servidor de AD FS principal
    *   Tipo en ```Netsh http show sslcert```
    *   Copiar el "GUID de aplicación" y el "hash de certificado" del servicio de Federación
    *   Tipo en `netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>Compruebe si el dispositivo cliente se ha aprovisionado correctamente con el certificado.
Es posible que observe que algunos dispositivos funcionan correctamente, pero otros no. En este caso, normalmente se debe a que el certificado de usuario no se ha aprovisionado correctamente en el dispositivo cliente. Siga estos pasos: 
1)  Si el problema es específico de un dispositivo Android, el problema más común es que la cadena de certificados no es de plena confianza en el dispositivo Android.  Consulte al proveedor de MDM para asegurarse de que el certificado se ha aprovisionado correctamente y de que toda la cadena es de plena confianza en el dispositivo Android. 
2)  Si el problema es específico de un dispositivo Windows, compruebe si el certificado se ha aprovisionado correctamente comprobando el almacén de certificados de Windows para el usuario que ha iniciado sesión (no sistema/equipo).
3)  Exporte el certificado de usuario cliente al archivo. cer y ejecute el comando "certutil-f-urlfetch-Verify certificatefilename. cer".


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>Compruebe si la versión de TLS es compatible entre los servidores AD FS/WAP y el dispositivo cliente.
En raras ocasiones, un dispositivo cliente (normalmente dispositivos móviles) se actualiza para admitir solo una versión superior de TLS (por ejemplo, 1,3) o puede tener el problema inverso en el que AD FS/servidores WAP se actualizaron para usar solo una versión de TLS superior y el dispositivo cliente no lo admite. Puede usar las herramientas de SSL en línea para comprobar los servidores de AD FS/WAP y ver si es compatible con el dispositivo. Para obtener más información sobre cómo controlar las versiones de TLS, consulte [este vínculo](manage-ssl-protocols-in-ad-fs.md).

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>Compruebe si Azure AD PromptLoginBehavior está configurado correctamente en la configuración del dominio federado.
Muchas aplicaciones de Office 365 envían prompt = login a Azure AD. De forma predeterminada Azure AD, lo convierte en un inicio de sesión de contraseña nueva en AD FS. Como resultado, incluso si ha configurado la autenticación de certificados en AD FS, los usuarios finales solo verán un inicio de sesión de contraseña. 
1)  Obtener la configuración de dominio federado mediante el comando "Get-MsolDomainFederationSettings"
2)  Asegúrese de que el parámetro PromptLoginBehavior está establecido en uno de los parámetros ' Disabled ' o ' NativeSupport '

Para obtener más información, consulte [este vínculo](ad-fs-prompt-login.md). 

### <a name="additional-troubleshooting"></a>Más soluciones de problemas
Se trata de casos poco frecuentes
1)  Si las listas de CRL son muy largas, puede agotar el tiempo de espera al intentar la descarga. En ese caso, debe actualizar "MaxFieldLength" y "MaxRequestByte" segúnhttps://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Referencia: lista completa de tipos de notificaciones de certificado de usuario y valores de ejemplo

|                                         Tipo de notificación                                         |                              Valor de ejemplo                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = EntCA, DC = Domain, DC = Contoso, DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = EntCA, DC = Domain, DC = Contoso, DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E = user@contoso.com , CN = user, CN = users, DC = Domain, DC = Contoso, DC = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E = user@contoso.com , CN = user, CN = users, DC = Domain, DC = Contoso, DC = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Datos de certificado digital codificados en Base64}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    ID. de la = D6 13 E3 6B BC E5 D8 15 52 0A FD 36 6A D5 0B 51 F3 0B 25 7F     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   Usuario                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | Otro nombre: nombre principal = user@contoso.com , nombre de RFC822 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>Vínculos relacionados
* [Configurar el enlace de nombre de host alternativo para la autenticación de AD FS certificado](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Configurar entidades de certificación en Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
