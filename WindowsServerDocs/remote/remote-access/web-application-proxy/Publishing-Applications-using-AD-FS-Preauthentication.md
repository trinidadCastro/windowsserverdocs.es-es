---
description: Más información acerca de cómo publicar aplicaciones mediante la autenticación previa de AD FS
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Publicación de aplicaciones con autenticación previa de AD FS
ms.author: kgremban
author: eross-msft
ms.date: 07/13/2016
ms.topic: article
ms.openlocfilehash: 774e621d1cc6a6449c758673a6bb1424ae3f2f17
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044933"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Publicación de aplicaciones con autenticación previa de AD FS

>Se aplica a: Windows Server 2016

**Este contenido es relevante para la versión local del proxy de aplicación Web. Para habilitar el acceso seguro a aplicaciones locales a través de la nube, consulte el [contenido del proxy de aplicación Azure ad](/azure/active-directory/manage-apps/application-proxy).**

En este tema se describe cómo publicar aplicaciones mediante el proxy de aplicación Web mediante la autenticación previa de Servicios de federación de Active Directory (AD FS) (AD FS).

En el caso de todos los tipos de aplicaciones que puede publicar mediante AD FS autenticación previa, debe agregar una AD FS relación de confianza para usuario autenticado con el Servicio de federación.

El flujo de autenticación previa AD FS general es el siguiente:

> [!NOTE]
> Este flujo de autenticación no es aplicable a los clientes que usan aplicaciones de Microsoft Store.

1.  El dispositivo cliente intenta obtener acceso a una aplicación web publicada en una dirección URL de recurso determinada; por ejemplo, https://app1.contoso.com/ .

    La dirección URL del recurso es una dirección pública en la que el proxy de aplicación web escucha las solicitudes HTTPS entrantes.

    Si está habilitada la redirección de HTTP a HTTPS, el proxy de aplicación web también escuchará las solicitudes HTTP entrantes.

2.  Proxy de aplicación web redirige la solicitud de HTTPS al servidor de AD FS con parámetros codificados de URL, incluida la dirección URL del recurso y el appRealm (un identificador de usuario de confianza).

    El usuario se autentica mediante el método de autenticación que requiere el servidor de AD FS; por ejemplo, nombre de usuario y contraseña, autenticación en dos fases con una contraseña de un solo tiempo, etc.

3.  Una vez autenticado el usuario, el servidor de AD FS emite un token de seguridad, el ' token perimetral ', que contiene la siguiente información y redirige la solicitud HTTPS de nuevo al servidor proxy de aplicación web:

    -   El identificador del recurso al cual el usuario ha intentado obtener acceso.

    -   La identidad del usuario como nombre principal de usuario (UPN).

    -   La expiración de la aprobación de concesión de acceso, es decir, se concede al usuario acceso por un periodo de tiempo limitado, tras el cual se le solicitará que se autentique de nuevo.

    -   Firma de la información en el token perimetral.

4.  El proxy de aplicación web recibe la solicitud de HTTPS redirigida desde el servidor de AD FS con el token perimetral y valida y usa el token de la siguiente manera:

    -   Valida que la firma del token perimetral sea del servicio de Federación configurado en la configuración del proxy de aplicación Web.

    -   Valida que el token haya sido emitido para la aplicación correcta.

    -   Valida que el token no haya expirado.

    -   Utiliza la identidad del usuario cuando sea necesario; por ejemplo para obtener un vale de Kerberos si el servidor back-end está configurado para utilizar la autenticación integrada de Windows.

5.  Si el token perimetral es válido, el proxy de aplicación web reenvía la solicitud de HTTPS a la aplicación web publicada mediante HTTP o HTTPS.

6.  El cliente ahora tiene acceso a la aplicación web publicada; no obstante, la aplicación web puede configurarse para requerir al usuario que realice una autenticación adicional. Si, por ejemplo, la aplicación web publicada es un sitio de SharePoint y no requiere autenticación adicional, el usuario verá el sitio de SharePoint en el explorador.

7.  El proxy de aplicación web guarda una cookie en el dispositivo cliente. El proxy de aplicación Web usa la cookie para identificar que esta sesión ya se ha autenticado previamente y que no es necesaria ninguna autenticación previa adicional.

> [!IMPORTANT]
> Al configurar la dirección URL externa y la dirección URL del servidor back-end, procure especificar el nombre de dominio completo (FQDN), no una dirección IP.

> [!NOTE]
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte [Uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="publish-a-claims-based-application-for-web-browser-clients"></a><a name="BKMK_1.1"></a>Publicar una aplicación basada en notificaciones para los clientes de explorador web
Para publicar una aplicación que utiliza notificaciones para la autenticación, debe agregar una relación de confianza para usuario autenticado para la aplicación al Servicio de federación.

Al publicar aplicaciones basadas en notificaciones y obtener acceso a la aplicación desde un explorador, el flujo de autenticación general es el siguiente:

1.  El cliente intenta obtener acceso a una aplicación basada en notificaciones mediante un explorador Web. por ejemplo, https://appserver.contoso.com/claimapp/ .

2.  El explorador Web envía una solicitud HTTPS al servidor proxy de aplicación web que redirige la solicitud al servidor de AD FS.

3.  El servidor de AD FS autentica al usuario y al dispositivo y redirige la solicitud de nuevo al proxy de aplicación Web. La solicitud ya contiene el token perimetral. El servidor de AD FS agrega una cookie de inicio de sesión único (SSO) a la solicitud porque el usuario ya ha realizado la autenticación en el servidor de AD FS.

4.  El proxy de aplicación web valida el token, agrega su propia cookie y reenvía la solicitud al servidor back-end.

5.  El servidor back-end redirige la solicitud al servidor de AD FS para obtener el token de seguridad de la aplicación.

6.  El servidor de AD FS redirige la solicitud al servidor back-end. La solicitud ya contiene el token de aplicación y el token de SSO. El usuario tiene acceso concedido a la aplicación y no es necesario que escriba un nombre de usuario o una contraseña.

En este procedimiento se describe cómo publicar una aplicación basada en notificaciones (como, por ejemplo, un sitio de SharePoint) a la que vayan a tener acceso los clientes de explorador web. Antes de comenzar, asegúrese de que ha hecho lo siguiente:

-   Creó una relación de confianza para usuario autenticado para la aplicación en la consola de administración de AD FS.

-   Compruebe que un certificado en el servidor proxy de aplicación web es adecuado para la aplicación que desea publicar.



### <a name="to-publish-a-claims-based-application"></a>Para publicar una aplicación basada en notificaciones

1.  En el servidor proxy de aplicación Web, en la consola de administración de acceso remoto, en el panel de **navegación** , haga clic en **proxy de aplicación web** y, a continuación, en el panel **tareas** , haga clic en **publicar**.

2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.

3.  En la página **autenticación previa** , haga clic en **servicios de Federación de Active Directory (AD FS) (AD FS)** y, a continuación, haga clic en **siguiente**.

4.  En la página **Clientes admitidos**, selecciona **Web y MSOFBA**, y después haz clic en **Siguiente**.

5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**.

6.  En la página **Configuración de publicación**, haga lo siguiente y, a continuación, haga clic en **Siguiente**:

    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.

        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.

    -   En el cuadro **Dirección URL externa**, indique la dirección URL externa de esta aplicación; por ejemplo, https://sp.contoso.com/app1/.

    -   En la lista **Certificado externo**, seleccione un certificado cuyo sujeto abarque la dirección URL externa.

    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se especifica automáticamente al escribir la dirección URL externa y debe cambiarla solo si la dirección URL del servidor back-end es diferente; por ejemplo, https://sp/app1/ .

        > [!NOTE]
        > El proxy de aplicación web puede traducir nombres de host en direcciones URL, pero no puede traducir nombres de ruta de acceso. Por lo tanto, se pueden especificar otros nombres de host, pero el nombre de la ruta de acceso debe ser el mismo. Por ejemplo, puede especificar una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://app-server/app1/ . Sin embargo, no puede especificar una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://apps.contoso.com/internal-app1/ .

7.  En la página **Confirmación**, revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.

8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.

#### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
Add-WebApplicationProxyApplication
    -BackendServerURL 'https://sp.contoso.com/app1/'
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'
    -ExternalURL 'https://sp.contoso.com/app1/'
    -Name 'SP'
    -ExternalPreAuthentication ADFS
    -ADFSRelyingPartyName 'SP_Relying_Party'
```

## <a name="publish-an-integrated-windows-authenticated-based-application-for-web-browser-clients"></a><a name="BKMK_1.2"></a>Publicar una aplicación basada en la autenticación integrada de Windows para clientes de explorador web
El proxy de aplicación web se puede usar para publicar aplicaciones que utilizan la autenticación integrada de Windows. es decir, el proxy de aplicación web realiza la autenticación previa según sea necesario y, a continuación, puede realizar el inicio de sesión único en la aplicación publicada que utiliza la autenticación integrada de Windows. Para publicar una aplicación que utiliza la autenticación integrada de Windows, debe agregar una relación de confianza para usuario autenticado no compatible con notificaciones para la aplicación al Servicio de federación.

Para permitir que el proxy de aplicación web realice el inicio de sesión único (SSO) y para realizar la delegación de credenciales mediante la delegación limitada de Kerberos, el servidor proxy de aplicación Web debe estar unido a un dominio. Consulte [Active Directory del plan](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)#BKMK_AD).

Para permitir que los usuarios accedan a las aplicaciones que usan la autenticación integrada de Windows, el servidor proxy de aplicación Web debe ser capaz de proporcionar delegación a los usuarios para la aplicación publicada. Puede hacerlo en el controlador de dominio para cualquier aplicación. También puede hacerlo en el servidor back-end si se está ejecutando en Windows Server 2012 R2 o Windows Server 2012. Para obtener más información, vea [novedades de la autenticación Kerberos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)).

Para ver un tutorial sobre cómo configurar el proxy de aplicación web para publicar una aplicación mediante la autenticación integrada de Windows, consulte [configurar un sitio para usar la autenticación integrada de Windows](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280943(v=ws.11)#BKMK_3).

Al usar la autenticación integrada de Windows en los servidores back-end, la autenticación entre el proxy de aplicación web y la aplicación publicada no está basada en notificaciones, sino que utiliza la delegación limitada de Kerberos para autenticar a los usuarios finales en la aplicación. El flujo general se describe a continuación:

1.  El cliente intenta obtener acceso a una aplicación no basada en notificaciones mediante un explorador Web. por ejemplo, https://appserver.contoso.com/nonclaimapp/ .

2.  El explorador Web envía una solicitud HTTPS al servidor proxy de aplicación web que redirige la solicitud al servidor de AD FS.

3.  El servidor de AD FS autentica al usuario y redirige la solicitud de nuevo al proxy de aplicación Web. La solicitud ya contiene el token perimetral.

4.  El proxy de aplicación web valida el token.

5.  Si el token es válido, el proxy de aplicación web obtiene un vale de Kerberos del controlador de dominio en nombre del usuario.

6.  El proxy de aplicación web agrega el vale de Kerberos a la solicitud como parte del token de mecanismo de negociación simple y protegida (SPNEGO) de GSS-API y reenvía la solicitud al servidor back-end. La solicitud contiene el vale de Kerberos; por consiguiente, se concede al usuario acceso a la aplicación sin que sea necesaria ninguna autenticación adicional.

En este procedimiento se describe cómo publicar una aplicación que usa la autenticación integrada de Windows (como, por ejemplo, Outlook Web App) a la que vayan a tener acceso los clientes de explorador web. Antes de comenzar, asegúrese de que ha hecho lo siguiente:

-   Crea una relación de confianza para usuario autenticado no compatible con notificaciones para la aplicación en la consola de administración de AD FS.

-   Ha configurado el servidor back-end para que admita la delegación limitada de Kerberos en el controlador de dominio usando el cmdlet Set-ADUser con el parámetro -PrincipalsAllowedToDelegateToAccount. Tenga en cuenta que si el servidor back-end se ejecuta en Windows Server 2012 R2 o Windows Server 2012, también puede ejecutar este comando de PowerShell en el servidor back-end.

-   Asegúrese de que los servidores proxy de aplicación Web estén configurados para la delegación en los nombres de entidad de seguridad de servicio de los servidores back-end.

-   Compruebe que un certificado en el servidor proxy de aplicación web es adecuado para la aplicación que desea publicar.



#### <a name="to-publish-a-non-claims-based-application"></a>Para publicar una aplicación que no esté basada en notificaciones

1.  En el servidor proxy de aplicación Web, en la consola de administración de acceso remoto, en el panel de **navegación** , haga clic en **proxy de aplicación web** y, a continuación, en el panel **tareas** , haga clic en **publicar**.

2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.

3.  En la página **autenticación previa** , haga clic en **servicios de Federación de Active Directory (AD FS) (AD FS)** y, a continuación, haga clic en **siguiente**.

4.  En la página **Clientes admitidos**, selecciona **Web y MSOFBA**, y después haz clic en **Siguiente**.

5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**.

6.  En la página **Configuración de publicación**, haga lo siguiente y, a continuación, haga clic en **Siguiente**:

    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.

        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.

    -   En el cuadro **Dirección URL externa**, indique la dirección URL externa de esta aplicación; por ejemplo, https://owa.contoso.com/.

    -   En la lista **Certificado externo**, seleccione un certificado cuyo sujeto abarque la dirección URL externa.

    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se especifica automáticamente al escribir la dirección URL externa y debe cambiarla solo si la dirección URL del servidor back-end es diferente; por ejemplo, https://owa/ .

        > [!NOTE]
        > El proxy de aplicación web puede traducir nombres de host en direcciones URL, pero no puede traducir nombres de ruta de acceso. Por lo tanto, se pueden especificar otros nombres de host, pero el nombre de la ruta de acceso debe ser el mismo. Por ejemplo, puede especificar una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://app-server/app1/ . Sin embargo, no puede especificar una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://apps.contoso.com/internal-app1/ .

    -   En el cuadro **SPN del servidor back-end**, escriba el nombre de entidad de seguridad de servicio del servidor back-end (por ejemplo, HTTP/owa.contoso.com).

7.  En la página **Confirmación**, revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.

8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.

#### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
Add-WebApplicationProxyApplication
    -BackendServerAuthenticationSpn 'HTTP/owa.contoso.com'
    -BackendServerURL 'https://owa.contoso.com/'
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'
    -ExternalURL 'https://owa.contoso.com/'
    -Name 'OWA'
    -ExternalPreAuthentication ADFS
    -ADFSRelyingPartyName 'Non-Claims_Relying_Party'
```

## <a name="publish-an-application-that-uses-ms-ofba"></a><a name="BKMK_1.3"></a>Publicación de una aplicación que usa MS-OFBA
Proxy de aplicación Web admite el acceso desde Microsoft Office clientes como Microsoft Word que acceden a documentos y datos en servidores back-end. La única diferencia entre estas aplicaciones y un explorador estándar es que el redireccionamiento al STS no se realiza a través de la redirección HTTP normal, sino con encabezados MS-OFBA especiales, como se especifica en: [https://msdn.microsoft.com/library/dd773463(v=office.12).aspx](/openspecs/sharepoint_protocols/ms-ofba/868d129f-f1b5-46bc-9385-4af58610dbbe) . La aplicación back-end pueden ser de notificaciones o IWA.
Para publicar una aplicación para los clientes que usan MS-OFBA, debe agregar una relación de confianza para usuario autenticado para la aplicación en el Servicio de federación. En función de la aplicación, puede utilizar la autenticación basada en notificaciones o la autenticación integrada de Windows. Por lo tanto, debe agregar la relación de confianza para usuario autenticado relevante en función de la aplicación.

Para permitir que el proxy de aplicación web realice el inicio de sesión único (SSO) y para realizar la delegación de credenciales mediante la delegación limitada de Kerberos, el servidor proxy de aplicación Web debe estar unido a un dominio. Consulte [Active Directory del plan](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)#BKMK_AD).

Si la aplicación usa la autenticación basada en notificaciones, no hay más pasos de planificación. Si la aplicación usa la autenticación integrada de Windows, consulte [publicación de una aplicación basada en autenticación integrada de Windows para clientes de explorador Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).

A continuación se describe el flujo de autenticación para los clientes que usan el protocolo MS-OFBA mediante la autenticación basada en notificaciones. La autenticación para este escenario puede utilizar el token de aplicación en la URL o en el cuerpo.

1.  El usuario está trabajando en un programa de Office y, desde la lista de **Documentos recientes**, abre un archivo en sitio de SharePoint.

2.  El programa de Office muestra una ventana con un control de explorador que requiere que el usuario introduzca credenciales.

    > [!NOTE]
    > En algunos casos, es posible que la ventana no aparezca porque el cliente ya está autenticado.

3.  El proxy de aplicación web redirige la solicitud al servidor de AD FS, que realiza la autenticación.

4.  El servidor de AD FS redirige la solicitud de nuevo al proxy de aplicación Web. La solicitud ya contiene el token perimetral.

5.  El servidor de AD FS agrega una cookie de inicio de sesión único (SSO) a la solicitud porque el usuario ya ha realizado la autenticación en el servidor de AD FS.

6.  El proxy de aplicación web valida el token y reenvía la solicitud al servidor back-end.

7.  El servidor back-end redirige la solicitud al servidor de AD FS para obtener el token de seguridad de la aplicación.

8.  La solicitud se redirige al servidor back-end. La solicitud ya contiene el token de aplicación y el token de SSO. El usuario tiene acceso concedido al sitio de SharePoint y no es necesario que escriba un nombre de usuario o una contraseña para ver el archivo.

Los pasos para publicar una aplicación que usa MS-OFBA son idénticos a los pasos para una aplicación basada en notificaciones o una aplicación no basada en notificaciones. En el caso de las aplicaciones basadas en notificaciones, consulte publicación de [una aplicación basada en notificaciones para clientes de explorador Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), para aplicaciones no basadas en notificaciones, consulte [publicación de una aplicación basada en autenticación integrada de Windows para clientes de explorador Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). El proxy de aplicación web detecta automáticamente el cliente y autenticará al usuario según sea necesario.

## <a name="publish-an-application-that-uses-http-basic"></a>Publicación de una aplicación que usa HTTP Basic

HTTP Basic es el protocolo de autorización usado por muchos protocolos, para conectar clientes enriquecidos, incluidos smartphones, con el buzón de Exchange. Para obtener más información sobre HTTP Basic, consulte [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). El proxy de aplicación Web suele interactuar con AD FS mediante redireccionamientos; los clientes más enriquecidos no admiten cookies ni administración de Estados. De esta manera, el proxy de aplicación web permite a la aplicación HTTP recibir una relación de confianza para usuario autenticado no notificaciones para la aplicación en el Servicio de federación. Consulte [Active Directory del plan](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)#BKMK_AD).

El flujo de autenticación para los clientes que usan HTTP Basic se describe a continuación y en este diagrama:

![Diagrama de autenticación para HTTP Basic](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)

1.  El usuario intenta obtener acceso a una aplicación web publicada en un cliente de teléfono.

2.  La aplicación envía una solicitud HTTPS a la dirección URL publicada por el proxy de aplicación Web.

3.  Si la solicitud no contiene credenciales, el proxy de aplicación Web devuelve una respuesta HTTP 401 a la aplicación que contiene la dirección URL del servidor de AD FS de autenticación.

4.  El usuario envía la solicitud HTTPS a la aplicación de nuevo con la autorización establecida en Basic y el nombre de usuario y la contraseña cifrada de base 64 del usuario en el encabezado de solicitud www-Authenticate.

5.  Dado que el dispositivo no se puede redirigir a AD FS, el proxy de aplicación Web envía una solicitud de autenticación a AD FS con las credenciales que incluye el nombre de usuario y la contraseña. El token se adquiere en nombre del dispositivo.

6.  Con el fin de minimizar el número de solicitudes enviadas al AD FS, el proxy de aplicación web valida las solicitudes de cliente posteriores mediante tokens almacenados en memoria caché mientras el token sea válido. El proxy de aplicación web limpia periódicamente la memoria caché. Puede ver el tamaño de la memoria caché mediante el contador de rendimiento.

7.  Si el token es válido, el proxy de aplicación web reenvía la solicitud al servidor back-end y se concede al usuario acceso a la aplicación web publicada.

En el procedimiento siguiente se explica cómo publicar aplicaciones HTTP Basic.

#### <a name="to-publish-an-http-basic-application"></a>Para publicar una aplicación HTTP Basic

1.  En el servidor proxy de aplicación Web, en la consola de administración de acceso remoto, en el panel de **navegación** , haga clic en **proxy de aplicación web** y, a continuación, en el panel **tareas** , haga clic en **publicar**.

2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.

3.  En la página **autenticación previa** , haga clic en **servicios de Federación de Active Directory (AD FS) (AD FS)** y, a continuación, haga clic en **siguiente**.

4.  En la página **clientes admitidos** , seleccione **http básico** y, a continuación, haga clic en **siguiente**.

    Si desea habilitar el acceso al intercambio solo desde dispositivos Unidos al área de trabajo, seleccione el cuadro **Habilitar acceso solo para dispositivos Unidos al área de trabajo** . Para obtener más información [, consulte unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en las aplicaciones de la empresa](../../../identity/ad-fs/operations/join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications.md).

5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**. Tenga en cuenta que esta lista contiene solo los usuarios de confianza de notificaciones.

6.  En la página **Configuración de publicación**, haga lo siguiente y, a continuación, haga clic en **Siguiente**:

    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.

        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.

    -   En el cuadro **dirección URL externa** , escriba la dirección URL externa de esta aplicación; por ejemplo, mail.contoso.com

    -   En la lista **Certificado externo**, seleccione un certificado cuyo sujeto abarque la dirección URL externa.

    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se especifica automáticamente al escribir la dirección URL externa y debe cambiarla solo si la dirección URL del servidor back-end es diferente; por ejemplo, mail.contoso.com.

7.  En la página **Confirmación**, revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.

8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.

#### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

Este script de Windows PowerShell permite la autenticación previa en todos los dispositivos, no solo en los dispositivos Unidos al área de trabajo.

```
Add-WebApplicationProxyApplication
     -BackendServerUrl 'https://mail.contoso.com'
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'
     -ExternalUrl 'https://mail.contoso.com'
     -Name 'Exchange'
     -ExternalPreAuthentication ADFSforRichClients
     -ADFSRelyingPartyName 'EAS_Relying_Party'
```

El siguiente solo realiza la autenticación de dispositivos Unidos al área de trabajo:

```
Add-WebApplicationProxyApplication
     -BackendServerUrl 'https://mail.contoso.com'
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'
     -EnableHTTPRedirect:$true
     -ExternalUrl 'https://mail.contoso.com'
     -Name 'Exchange'
     -ExternalPreAuthentication ADFSforRichClients
     -ADFSRelyingPartyName 'EAS_Relying_Party'
```

## <a name="publish-an-application-that-uses-oauth2-such-as-a-microsoft-store-app"></a><a name="BKMK_1.4"></a>Publicación de una aplicación que usa OAuth2, como una aplicación Microsoft Store
Para publicar una aplicación para Microsoft Store aplicaciones, debe agregar una relación de confianza para usuario autenticado para la aplicación en el Servicio de federación.

Para permitir que el proxy de aplicación web realice el inicio de sesión único (SSO) y para realizar la delegación de credenciales mediante la delegación limitada de Kerberos, el servidor proxy de aplicación Web debe estar unido a un dominio. Consulte [Active Directory del plan](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)#BKMK_AD).

> [!NOTE]
> Proxy de aplicación Web admite la publicación solo para Microsoft Store aplicaciones que usan el protocolo OAuth 2,0.

En la consola de administración de AD FS, debe asegurarse de que el extremo de OAuth está habilitado para proxy. Para comprobar si el punto final OAuth está habilitado para proxy, abra la Consola de administración de AD FS, expanda **Servicio**, haga clic en **Puntos finales**, en la lista **Puntos finales**, localice el punto final OAuth y asegúrese de que el valor de la columna **Habilitado para proxy** sea **Sí**.

A continuación se describe el flujo de autenticación para los clientes que usan aplicaciones de Microsoft Store:

> [!NOTE]
> El proxy de aplicación web redirige al servidor de AD FS para la autenticación. Dado que Microsoft Store aplicaciones no admiten redirecciones, si usa Microsoft Store aplicaciones, es necesario establecer la dirección URL del servidor de AD FS con el cmdlet Set-WebApplicationProxyConfiguration y el parámetro OAuthAuthenticationURL.
>
> Microsoft Store aplicaciones solo se pueden publicar con Windows PowerShell.

1.  El cliente intenta obtener acceso a una aplicación web publicada con una aplicación Microsoft Store.

2.  La aplicación envía una solicitud HTTPS a la dirección URL publicada por el proxy de aplicación Web.

3.  Proxy de aplicación Web devuelve una respuesta HTTP 401 a la aplicación que contiene la dirección URL del servidor de AD FS de autenticación. Este proceso se conoce como "detección".

    > [!NOTE]
    > Si la aplicación conoce la dirección URL del servidor de AD FS de autenticación y ya tiene un token combinado que contiene el token de OAuth y el token perimetral, los pasos 2 y 3 se omiten en este flujo de autenticación.

4.  La aplicación envía una solicitud HTTPS al servidor de AD FS.

5.  La aplicación usa el agente de autenticación Web para generar un cuadro de diálogo en el que el usuario escribe las credenciales para autenticarse en el servidor de AD FS. Para obtener información sobre el agente de autenticación web, consulta [Agente de autenticación web](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)).

6.  Después de la autenticación correcta, el servidor de AD FS crea un token combinado que contiene el token OAuth y el token perimetral y envía el token a la aplicación.

7.  La aplicación envía una solicitud HTTPS que contiene el token combinado a la dirección URL publicada por el proxy de aplicación Web.

8.  El proxy de aplicación web divide el token combinado en sus dos partes y valida el token perimetral.

9. Si el token perimetral es válido, el proxy de aplicación web reenvía la solicitud al servidor back-end con solo el token de OAuth. Se concede al usuario acceso a la aplicación web publicada.

En este procedimiento se explica cómo publicar una aplicación para OAuth2. Este tipo de aplicación solo se puede publicar mediante Windows PowerShell. Antes de comenzar, asegúrese de que ha hecho lo siguiente:

-   Creó una relación de confianza para usuario autenticado para la aplicación en la consola de administración de AD FS.

-   Asegúrese de que el extremo de OAuth está habilitado para proxy en la consola de administración de AD FS y anote la ruta de acceso de la dirección URL.

-   Compruebe que un certificado en el servidor proxy de aplicación web es adecuado para la aplicación que desea publicar.

#### <a name="to-publish-an-oauth2-app"></a>Para publicar una aplicación OAuth2

1.  En el servidor proxy de aplicación Web, en la consola de administración de acceso remoto, en el panel de **navegación** , haga clic en **proxy de aplicación web** y, a continuación, en el panel **tareas** , haga clic en **publicar**.

2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.

3.  En la página **autenticación previa** , haga clic en **servicios de Federación de Active Directory (AD FS) (AD FS)** y, a continuación, haga clic en **siguiente**.

4.  En la página **clientes admitidos** , seleccione **OAuth2** y, a continuación, haga clic en **siguiente**.

5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**.

6.  En la página **Configuración de publicación**, haga lo siguiente y, a continuación, haga clic en **Siguiente**:

    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.

        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.

    -   En el cuadro **Dirección URL externa**, indique la dirección URL externa de esta aplicación; por ejemplo, https://server1.contoso.com/app1/.

    -   En la lista **Certificado externo**, seleccione un certificado cuyo sujeto abarque la dirección URL externa.

        Para asegurarse de que los usuarios puedan tener acceso a la aplicación, aunque no tengan que escribir HTTPS en la dirección URL, seleccione el cuadro **Habilitar redirección de http a https** .

    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se especifica automáticamente al escribir la dirección URL externa y debe cambiarla solo si la dirección URL del servidor back-end es diferente; por ejemplo, https://sp/app1/ .

        > [!NOTE]
        > El proxy de aplicación web puede traducir nombres de host en direcciones URL, pero no puede traducir nombres de ruta de acceso. Por lo tanto, se pueden especificar otros nombres de host, pero el nombre de la ruta de acceso debe ser el mismo. Por ejemplo, puede especificar una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://app-server/app1/ . Sin embargo, no puede especificar una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://apps.contoso.com/internal-app1/ .

7.  En la página **Confirmación**, revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.

8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.

Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

Para establecer la dirección URL de autenticación de OAuth para una dirección de servidor de Federación de fs.contoso.com y una ruta de acceso de dirección URL de/ADFS/OAuth2/:

```
Set-WebApplicationProxyConfiguration -OAuthAuthenticationURL 'https://fs.contoso.com/adfs/oauth2/'
```

Para publicar la aplicación:

```
Add-WebApplicationProxyApplication
    -BackendServerURL 'https://storeapp.contoso.com/'
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'
    -ExternalURL 'https://storeapp.contoso.com/'
    -Name 'Microsoft Store app Server'
    -ExternalPreAuthentication ADFS
    -ADFSRelyingPartyName 'Store_app_Relying_Party'
    -UseOAuthAuthentication
```

## <a name="see-also"></a><a name="BKMK_Links"></a>Otras referencias

-   [Solución de problemas del Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

-   [Publicación de aplicaciones mediante el proxy de aplicación Web](/previous-versions/orphan-topics/ws.11/dn383659(v=ws.11))

-   [Planeamiento de publicación de aplicaciones mediante el Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))

-   [Guía de tutorial de proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))

-   [Cmdlets del Proxy de aplicación web en Windows PowerShell](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))

-   [Add-WebApplicationProxyApplication](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))

-   [Set-WebApplicationProxyConfiguration](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))

