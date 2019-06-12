---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Publicación de aplicaciones con autenticación previa de AD FS
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: ca4d8661f8f0252334bdecbde85603d8af5e2d2a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446814"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Publicación de aplicaciones con autenticación previa de AD FS

>Se aplica a: Windows Server 2016

**Este contenido es relevante para la versión local del Proxy de aplicación Web. Para habilitar el acceso seguro a aplicaciones locales a través de la nube, consulte el [contenido de Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Este tema describe cómo publicar aplicaciones mediante el Proxy de aplicación Web mediante Active Directory Federation Services (AD FS) la autenticación previa.  
  
Para todos los tipos de aplicación que se puede publicar mediante la autenticación previa de AD FS, debe agregar un usuario de confianza de confianza para el servicio de federación de AD FS.  
  
El flujo general de la autenticación previa de AD FS es como sigue:  
  
> [!NOTE]  
> Este flujo de autenticación no es aplicable para clientes que utilizan aplicaciones de Microsoft Store.  
  
1.  El dispositivo cliente intenta obtener acceso a una aplicación web publicada en una dirección URL de recurso en particular; Por ejemplo https://app1.contoso.com/.  
  
    La dirección URL de recursos es una dirección pública en el que el Proxy de aplicación Web escucha las solicitudes HTTPS entrantes.  
  
    Si está habilitado HTTP al redireccionamiento de HTTPS, los Proxy de aplicación Web también escuchará las solicitudes HTTP entrantes.  
  
2.  Proxy de aplicación Web redirige la solicitud de HTTPS al servidor de AD FS con parámetros de dirección URL codificada, incluyendo la URL de recursos y el appRealm (un identificador del usuario autenticado).  
  
    El usuario autentica mediante el método de autenticación requerido por el servidor de AD FS; Por ejemplo, nombre de usuario y contraseña, autenticación en dos fases con una contraseña temporal y así sucesivamente.  
  
3.  Cuando el usuario está autenticado, el servidor de AD FS emite una seguridad token, el 'token perimetral', que contiene la información siguiente y redirige la solicitud de HTTPS al servidor Proxy de aplicación Web:  
  
    -   El identificador del recurso al cual el usuario ha intentado obtener acceso.  
  
    -   La identidad del usuario como un nombre principal de usuario (UPN).  
  
    -   La expiración de la aprobación de concesión de acceso, es decir, se concede al usuario acceso por un periodo de tiempo limitado, tras el cual se le solicitará que se autentique de nuevo.  
  
    -   Firma de la información en el token perimetral.  
  
4.  Proxy de aplicación Web recibe una solicitud HTTPS redirigida desde el servidor de AD FS con el token perimetral y valida y utiliza el token de la manera siguiente:  
  
    -   Valida que la firma del token perimetral es el servicio de federación que está configurado en la configuración de Proxy de aplicación Web.  
  
    -   Valida que el token haya sido emitido para la aplicación correcta.  
  
    -   Valida que el token no haya expirado.  
  
    -   Utiliza la identidad del usuario cuando sea necesario; por ejemplo para obtener un vale de Kerberos si el servidor back-end está configurado para utilizar la autenticación integrada de Windows.  
  
5.  Si el token perimetral es válido, el Proxy de aplicación Web reenvía la solicitud HTTPS a la aplicación web publicada utilizando HTTP o HTTPS.  
  
6.  El cliente ahora tiene acceso a la aplicación web publicada; no obstante, la aplicación web puede configurarse para requerir al usuario que realice una autenticación adicional. Si, por ejemplo, la aplicación web publicada es un sitio de SharePoint y no requiere autenticación adicional, el usuario verá el sitio de SharePoint en el explorador.  
  
7.  Proxy de aplicación Web se guarda una cookie en el dispositivo cliente. La cookie se usa por el Proxy de aplicación Web para identificar que esta sesión ya ha sido autenticada y que no es necesaria ninguna autenticación previa adicional.  
  
> [!IMPORTANT]  
> Al configurar la dirección URL externa y la dirección URL del servidor back-end, procure especificar el nombre de dominio completo (FQDN), no una dirección IP.  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>Publicar una aplicación basada en notificaciones para clientes de explorador Web  
Para publicar una aplicación que utiliza notificaciones para la autenticación, debe agregar una relación de confianza para usuario autenticado para la aplicación al Servicio de federación.  
  
Al publicar aplicaciones basadas en notificaciones y obtener acceso a la aplicación desde un explorador, el flujo de autenticación general es el siguiente:  
  
1.  El cliente intenta obtener acceso a una aplicación basada en notificaciones mediante un explorador web; Por ejemplo, https://appserver.contoso.com/claimapp/.  
  
2.  El explorador web envía una solicitud HTTPS al servidor Proxy de aplicación Web que se redirige la solicitud al servidor de AD FS.  
  
3.  El servidor de AD FS autentica el usuario y el dispositivo y redirige la solicitud al Proxy de aplicación Web. La solicitud ya contiene el token perimetral. El servidor de AD FS agrega una cookie única inicio de sesión (SSO) a la solicitud porque el usuario ya ha efectuado la autenticación en el servidor de AD FS.  
  
4.  Proxy de aplicación Web valida el token, agrega su propia cookie y reenvía la solicitud al servidor back-end.  
  
5.  El servidor back-end redirige la solicitud al servidor de AD FS para la seguridad de la aplicación obtener el token.  
  
6.  La solicitud se redirige al servidor back-end y el servidor de AD FS. La solicitud ya contiene el token de aplicación y el token de SSO. El usuario tiene acceso concedido a la aplicación y no es necesario que escriba un nombre de usuario o una contraseña.  
  
En este procedimiento se describe cómo publicar una aplicación basada en notificaciones (como, por ejemplo, un sitio de SharePoint) a la que vayan a tener acceso los clientes de explorador web. Antes de comenzar, asegúrese de que ha hecho lo siguiente:  
  
-   Crea una confianza para la aplicación en la consola de administración de AD FS.  
  
-   Comprobar que un certificado en el servidor Proxy de aplicación Web es adecuado para la aplicación que desea publicar.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Para publicar una aplicación basada en notificaciones  
  
1.  En el servidor Proxy de aplicación Web, en la consola de administración de acceso remoto, en el **navegación** panel, haga clic en **Proxy de aplicación Web**y, a continuación, en el **tareas** panel, haga clic en **Publicar**.  
  
2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.  
  
3.  En el **la autenticación previa** página, haga clic en **los servicios de federación de Active Directory (AD FS)** y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Clientes admitidos** , selecciona **Web y MSOFBA**, y después haz clic en **Siguiente**.  
  
5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**.  
  
6.  En la página **Configuración de publicación** , haga lo siguiente y, a continuación, haga clic en **Siguiente**:  
  
    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.  
  
        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.  
  
    -   En el cuadro **Dirección URL externa**, indique la dirección URL externa de esta aplicación; por ejemplo, https://sp.contoso.com/app1/.  
  
    -   En la lista **Certificado externo** , seleccione un certificado cuyo sujeto abarque la dirección URL externa.  
  
    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se introduce automáticamente al escribir la dirección URL externa y debe cambiarlo solo si la dirección URL del servidor back-end es diferente; Por ejemplo, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy de aplicación Web puede traducir los nombres de host en direcciones URL, pero no los nombres de ruta. Por lo tanto, se pueden especificar otros nombres de host, pero el nombre de la ruta de acceso debe ser el mismo. Por ejemplo, puede escribir una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://app-server/app1/. Sin embargo, no se puede escribir una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://apps.contoso.com/internal-app1/.  
  
7.  En la página **Confirmación** , revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.  
  
8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Comandos equivalentes de Windows PowerShell</em>***  
  
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
  
## <a name="BKMK_1.2"></a>Publicar una aplicación basada en autenticado de Windows integrada para clientes de explorador Web  
Proxy de aplicación Web puede usarse para publicar aplicaciones que utilizan la autenticación de Windows integrada, es decir, el Proxy de aplicación Web realiza la autenticación previa según sea necesario y, a continuación, se puede realizar el inicio de sesión único en la aplicación publicada que utiliza la autenticación de Windows integrada. Para publicar una aplicación que utiliza la autenticación integrada de Windows, debe agregar una relación de confianza para usuario autenticado no compatible con notificaciones para la aplicación al Servicio de federación.  
  
Para permitir que el Proxy de aplicación Web para realizar el inicio de sesión único (SSO) y para realizar la delegación de credenciales con Kerberos la delegación restringida, el Proxy de aplicación Web server debe estar unido a un dominio. Consulte [planear Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Para permitir que los usuarios accedan a las aplicaciones que usan la autenticación de Windows integrada, el servidor Proxy de aplicación Web debe poder proporcionar delegación para los usuarios en la aplicación publicada. Puede hacerlo en el controlador de dominio para cualquier aplicación. También puede hacerlo en el servidor back-end si se está ejecutando en Windows Server 2012 R2 o Windows Server 2012. Para más información, consulte [Novedades de la autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Para ver un tutorial sobre cómo configurar el Proxy de aplicación Web para publicar una aplicación mediante la autenticación de Windows integrada, vea [configurar un sitio para usar la autenticación de Windows integrada](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Al utilizar la autenticación de Windows integrada para servidores back-end, la autenticación entre el Proxy de aplicación Web y la aplicación publicada no está basada en notificaciones, sino que utiliza la delegación restringida de Kerberos para autenticar a los usuarios finales a la aplicación. El flujo general se describe a continuación:  
  
1.  El cliente intenta obtener acceso a una aplicación no basada en notificaciones mediante un explorador web; Por ejemplo, https://appserver.contoso.com/nonclaimapp/.  
  
2.  El explorador web envía una solicitud HTTPS al servidor Proxy de aplicación Web que se redirige la solicitud al servidor de AD FS.  
  
3.  El servidor de AD FS autentica al usuario y redirige la solicitud al Proxy de aplicación Web. La solicitud ya contiene el token perimetral.  
  
4.  Proxy de aplicación Web valida el token.  
  
5.  Si el token es válido, el Proxy de aplicación Web obtiene un vale Kerberos del controlador de dominio en nombre del usuario.  
  
6.  Proxy de aplicación Web agrega el vale de Kerberos para la solicitud como parte del token Simple y el mecanismo de negociación GSS-API protegida (SPNEGO) y reenvía la solicitud al servidor back-end. La solicitud contiene el vale de Kerberos; por consiguiente, se concede al usuario acceso a la aplicación sin que sea necesaria ninguna autenticación adicional.  
  
En este procedimiento se describe cómo publicar una aplicación que usa la autenticación integrada de Windows (como, por ejemplo, Outlook Web App) a la que vayan a tener acceso los clientes de explorador web. Antes de comenzar, asegúrese de que ha hecho lo siguiente:  
  
-   Crea una que no es compatible con notificaciones de confianza para la aplicación en la consola de administración de AD FS.  
  
-   Ha configurado el servidor back-end para que admita la delegación limitada de Kerberos en el controlador de dominio usando el cmdlet Set-ADUser con el parámetro -PrincipalsAllowedToDelegateToAccount. Tenga en cuenta que si el servidor back-end se ejecuta en Windows Server 2012 R2 o Windows Server 2012, también puede ejecutar este comando de PowerShell en el servidor back-end.  
  
-   Se aseguró de que los servidores Proxy de aplicación Web se configuran para la delegación a los nombres de entidad de servicio de los servidores back-end.  
  
-   Comprobar que un certificado en el servidor Proxy de aplicación Web es adecuado para la aplicación que desea publicar.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Para publicar una aplicación que no esté basada en notificaciones  
  
1.  En el servidor Proxy de aplicación Web, en la consola de administración de acceso remoto, en el **navegación** panel, haga clic en **Proxy de aplicación Web**y, a continuación, en el **tareas** panel, haga clic en **Publicar**.  
  
2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.  
  
3.  En el **la autenticación previa** página, haga clic en **los servicios de federación de Active Directory (AD FS)** y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Clientes admitidos** , selecciona **Web y MSOFBA**, y después haz clic en **Siguiente**.  
  
5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**.  
  
6.  En la página **Configuración de publicación** , haga lo siguiente y, a continuación, haga clic en **Siguiente**:  
  
    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.  
  
        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.  
  
    -   En el cuadro **Dirección URL externa**, indique la dirección URL externa de esta aplicación; por ejemplo, https://owa.contoso.com/.  
  
    -   En la lista **Certificado externo** , seleccione un certificado cuyo sujeto abarque la dirección URL externa.  
  
    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se introduce automáticamente al escribir la dirección URL externa y debe cambiarlo solo si la dirección URL del servidor back-end es diferente; Por ejemplo, https://owa/.  
  
        > [!NOTE]  
        > Proxy de aplicación Web puede traducir los nombres de host en direcciones URL, pero no los nombres de ruta. Por lo tanto, se pueden especificar otros nombres de host, pero el nombre de la ruta de acceso debe ser el mismo. Por ejemplo, puede escribir una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://app-server/app1/. Sin embargo, no se puede escribir una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://apps.contoso.com/internal-app1/.  
  
    -   En el cuadro **SPN del servidor back-end**, escriba el nombre de entidad de seguridad de servicio del servidor back-end (por ejemplo, HTTP/owa.contoso.com).  
  
7.  En la página **Confirmación** , revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.  
  
8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Comandos equivalentes de Windows PowerShell</em>***  
  
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
  
## <a name="BKMK_1.3"></a>Publicar una aplicación que usa MS-OFBA  
Proxy de aplicación Web admite el acceso desde clientes de Microsoft Office como Microsoft Word que obtienen acceso a documentos y datos en servidores back-end. La única diferencia entre estas aplicaciones y un explorador estándar es que se realiza la redirección al STS no mediante un redireccionamiento HTTP normal sino con encabezados como se especifica en MS-OFBA: [ https://msdn.microsoft.com/library/dd773463(v=office.12).aspx ](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). La aplicación back-end pueden ser de notificaciones o IWA.   
Para publicar una aplicación para clientes que usan MS-OFBA, debe agregar una usuario de confianza de confianza para la aplicación al servicio de federación. En función de la aplicación, puede utilizar la autenticación basada en notificaciones o la autenticación integrada de Windows. Por lo tanto, debe agregar la relación de confianza para usuario autenticado relevante en función de la aplicación.  
  
Para permitir que el Proxy de aplicación Web para realizar el inicio de sesión único (SSO) y para realizar la delegación de credenciales con Kerberos la delegación restringida, el Proxy de aplicación Web server debe estar unido a un dominio. Consulte [planear Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Si la aplicación usa la autenticación basada en notificaciones, no hay más pasos de planificación. Si la aplicación utiliza la autenticación de Windows integrada, consulte [publicar una aplicación basada en autenticado de Windows integrada para clientes de explorador Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
A continuación, se describe el flujo de autenticación para clientes que usan el protocolo MS-OFBA mediante la autenticación basada en notificaciones. La autenticación para este escenario puede utilizar el token de aplicación en la URL o en el cuerpo.  
  
1.  El usuario está trabajando en un programa de Office y, desde la lista de **Documentos recientes**, abre un archivo en sitio de SharePoint.  
  
2.  El programa de Office muestra una ventana con un control de explorador que requiere que el usuario introduzca credenciales.  
  
    > [!NOTE]  
    > En algunos casos, es posible que la ventana no aparezca porque el cliente ya está autenticado.  
  
3.  Proxy de aplicación Web redirige la solicitud al servidor de AD FS, que realiza la autenticación.  
  
4.  El servidor de AD FS redirige la solicitud a Proxy de aplicación Web. La solicitud ya contiene el token perimetral.  
  
5.  El servidor de AD FS agrega una cookie única inicio de sesión (SSO) a la solicitud porque el usuario ya ha efectuado la autenticación en el servidor de AD FS.  
  
6.  Proxy de aplicación Web valida el token y reenvía la solicitud al servidor back-end.  
  
7.  El servidor back-end redirige la solicitud al servidor de AD FS para la seguridad de la aplicación obtener el token.  
  
8.  La solicitud se redirige al servidor back-end. La solicitud ya contiene el token de aplicación y el token de SSO. El usuario tiene acceso concedido al sitio de SharePoint y no es necesario que escriba un nombre de usuario o una contraseña para ver el archivo.  
  
Los pasos necesarios para publicar una aplicación que usa MS-OFBA son idénticos a los pasos para una aplicación basada en notificaciones o una aplicación no basada en notificaciones. Para las aplicaciones basadas en notificaciones, consulte [publicar una aplicación basada en notificaciones para clientes de explorador Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), para las aplicaciones no basadas en notificaciones, consulte [publicar una aplicación basada en autenticado de Windows integrada para Web Los clientes de explorador](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). Proxy de aplicación Web detecta automáticamente el cliente y autenticará al usuario según sea necesario.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Publicar una aplicación que utiliza Basic de HTTP  

HTTP básico es el protocolo de autorización utilizado por muchos de los protocolos, para conectarse a los clientes enriquecidos, incluidos los smartphones, con el buzón de Exchange. Para obtener más información sobre HTTP básico, consulte [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). Proxy de aplicación Web tradicionalmente interactúa con AD FS mediante redirecciones; la mayoría de los clientes enriquecidos no son compatibles con las cookies o administración de Estados. De este modo Proxy de aplicación Web permite que la aplicación HTTP recibir una no basada en notificaciones de confianza para usuario autenticado para la aplicación al servicio de federación. Consulte [planear Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
El flujo de autenticación para clientes que usan HTTP básica se describe a continuación y en este diagrama:  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  El usuario intenta tener acceso a una aplicación web publicada un cliente de teléfono.  
  
2.  La aplicación envía una solicitud HTTPS a la dirección URL publicada por el Proxy de aplicación Web.  
  
3.  Si la solicitud no contiene las credenciales, el Proxy de aplicación Web devuelve una respuesta HTTP 401 a la aplicación que contiene la dirección URL del servidor de AD FS de autenticación.  
  
4.  El usuario envía la solicitud HTTPS a la aplicación de nuevo con autorización establecida en Basic y nombre de usuario y Base 64 contraseña cifrada del usuario en el World Wide Web-del encabezado de solicitud de autenticación.  
  
5.  Dado que el dispositivo no se pueden redirigir a AD FS, el Proxy de aplicación Web envía una solicitud de autenticación para AD FS con las credenciales que tiene como nombre de usuario y la contraseña. El token se adquiere en nombre del dispositivo.  
  
6.  Con el fin de minimizar el número de solicitudes enviadas a la instancia de AD FS, Proxy de aplicación Web, valida las solicitudes de cliente posteriores con tokens en caché para siempre y cuando el token es válido. Proxy de aplicación Web periódicamente limpia la memoria caché. Puede ver el tamaño de la memoria caché mediante el contador de rendimiento.  
  
7.  Si el token es válido, el Proxy de aplicación Web reenvía la solicitud al servidor back-end y se concede al usuario acceso a la aplicación web publicada.  
  
El siguiente procedimiento explica cómo publicar aplicaciones básicas de HTTP.  
  
#### <a name="to-publish-an-http-basic-application"></a>Para publicar una aplicación básica de HTTP  
  
1.  En el servidor Proxy de aplicación Web, en la consola de administración de acceso remoto, en el **navegación** panel, haga clic en **Proxy de aplicación Web**y, a continuación, en el **tareas** panel, haga clic en **Publicar**.  
  
2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.  
  
3.  En el **la autenticación previa** página, haga clic en **los servicios de federación de Active Directory (AD FS)** y, a continuación, haga clic en **siguiente**.  
  
4.  En el **clientes admitidos** página, seleccione **HTTP Basic** y, a continuación, haga clic en **siguiente**.  
  
    Si desea habilitar el acceso a Exchange solo desde dispositivos Unidos a un área de trabajo, seleccione el **dispositivos Unidos a habilitar el acceso sólo para workplace** cuadro. Para obtener más información, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y sin problemas segundo Factor Authentication Across Company Applications](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**. Tenga en cuenta que esta lista contiene solo usuarios autenticados en notificaciones.  
  
6.  En la página **Configuración de publicación** , haga lo siguiente y, a continuación, haga clic en **Siguiente**:  
  
    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.  
  
        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.  
  
    -   En el **dirección URL externa** , escriba la dirección URL externa de esta aplicación; por ejemplo, mail.contoso.com  
  
    -   En la lista **Certificado externo** , seleccione un certificado cuyo sujeto abarque la dirección URL externa.  
  
    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se introduce automáticamente al escribir la dirección URL externa y debe cambiarlo solo si la dirección URL del servidor back-end es diferente; Por ejemplo, mail.contoso.com.  
  
7.  En la página **Confirmación** , revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.  
  
8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Este script de Windows PowerShell permite la autenticación previa para todos los dispositivos, no solo los dispositivos Unidos a un área de trabajo.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
El siguiente autentica previamente solo dispositivos Unidos a un área de trabajo:  
  
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
  
## <a name="BKMK_1.4"></a>Publicar una aplicación que usa OAuth2 como una aplicación de Microsoft Store  
Para publicar una aplicación para aplicaciones de Microsoft Store, debe agregar una usuario de confianza de confianza para la aplicación al servicio de federación.  
  
Para permitir que el Proxy de aplicación Web para realizar el inicio de sesión único (SSO) y para realizar la delegación de credenciales con Kerberos la delegación restringida, el Proxy de aplicación Web server debe estar unido a un dominio. Consulte [planear Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> Proxy de aplicación Web admite la publicación únicamente para las aplicaciones de Microsoft Store que usan el protocolo OAuth 2.0.  
  
En la consola de administración de AD FS, debe asegurarse de que el punto final OAuth está habilitado para proxy. Para comprobar si el punto final OAuth está habilitado para proxy, abra la Consola de administración de AD FS, expanda **Servicio**, haga clic en **Puntos finales**, en la lista **Puntos finales** , localice el punto final OAuth y asegúrese de que el valor de la columna **Habilitado para proxy** sea **Sí**.  
  
El flujo de autenticación para clientes que utilizan aplicaciones de Microsoft Store se describe a continuación:  
  
> [!NOTE]  
> El Proxy de aplicación Web redirige al servidor de AD FS para la autenticación. Dado que las aplicaciones de Microsoft Store no admiten redireccionamientos, si usa aplicaciones de Microsoft Store, es necesario establecer la dirección URL del servidor de AD FS mediante el cmdlet Set-WebApplicationProxyConfiguration y el parámetro OAuthAuthenticationURL.  
>   
> Las aplicaciones de Microsoft Store se pueden publicar solo mediante Windows PowerShell.  
  
1.  El cliente intenta tener acceso a una aplicación web publicada con una aplicación de Microsoft Store.  
  
2.  La aplicación envía una solicitud HTTPS a la dirección URL publicada por el Proxy de aplicación Web.  
  
3.  Proxy de aplicación Web devuelve una respuesta HTTP 401 a la aplicación que contiene la dirección URL del servidor de AD FS de autenticación. Este proceso se conoce como 'descubrimiento'.  
  
    > [!NOTE]  
    > Si la aplicación conoce la dirección URL del servidor de AD FS de autenticación y ya tiene un token combinado que contiene el token OAuth y el token perimetral, los pasos 2 y 3 se omiten en este flujo de autenticación.  
  
4.  La aplicación envía una solicitud HTTPS al servidor de AD FS.  
  
5.  La aplicación utiliza al agente de autenticación web para generar un cuadro de diálogo en el que el usuario introduce credenciales para autenticarse en el servidor de AD FS. Para obtener información sobre el agente de autenticación web, consulta [Agente de autenticación web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Tras una autenticación correcta, el servidor de AD FS crea un token combinado que contiene el token de OAuth y el token perimetral y envía el token a la aplicación.  
  
7.  La aplicación envía una solicitud HTTPS que contiene el token combinado a la dirección URL publicada por el Proxy de aplicación Web.  
  
8.  Proxy de aplicación Web se divide el token combinado en sus dos partes y valida el token perimetral.  
  
9. Si el token perimetral es válido, el Proxy de aplicación Web reenvía la solicitud al servidor back-end con únicamente el token de OAuth. Se concede al usuario acceso a la aplicación web publicada.  
  
En este procedimiento se explica cómo publicar una aplicación para OAuth2. Este tipo de aplicación se puede publicar solo mediante Windows PowerShell. Antes de comenzar, asegúrese de que ha hecho lo siguiente:  
  
-   Crea una confianza para la aplicación en la consola de administración de AD FS.  
  
-   Se aseguró de que el punto final OAuth está habilitado para proxy en la consola de administración de AD FS y tome nota de la ruta de acceso de dirección URL.  
  
-   Comprobar que un certificado en el servidor Proxy de aplicación Web es adecuado para la aplicación que desea publicar.  
  
#### <a name="to-publish-an-oauth2-app"></a>Para publicar una aplicación de OAuth2  
  
1.  En el servidor Proxy de aplicación Web, en la consola de administración de acceso remoto, en el **navegación** panel, haga clic en **Proxy de aplicación Web**y, a continuación, en el **tareas** panel, haga clic en **Publicar**.  
  
2.  En el **Asistente para publicar nuevas aplicaciones**, en el cuadro de diálogo **Página principal**, haga clic en **Siguiente**.  
  
3.  En el **la autenticación previa** página, haga clic en **los servicios de federación de Active Directory (AD FS)** y, a continuación, haga clic en **siguiente**.  
  
4.  En el **clientes admitidos** página, seleccione **OAuth2** y, a continuación, haga clic en **siguiente**.  
  
5.  En la lista de usuarios de confianza de la página **Persona de confianza**, seleccione el usuario de confianza de la aplicación que desea publicar y haga clic en **Siguiente**.  
  
6.  En la página **Configuración de publicación** , haga lo siguiente y, a continuación, haga clic en **Siguiente**:  
  
    -   En el cuadro **Nombre**, escriba un nombre descriptivo para la aplicación.  
  
        Este nombre se usa únicamente en la lista de aplicaciones publicadas de la Consola de administración de acceso remoto.  
  
    -   En el cuadro **Dirección URL externa**, indique la dirección URL externa de esta aplicación; por ejemplo, https://server1.contoso.com/app1/.  
  
    -   En la lista **Certificado externo** , seleccione un certificado cuyo sujeto abarque la dirección URL externa.  
  
        Con el fin de asegurarse de que los usuarios pueden acceder a la aplicación, incluso si olvidó escribir la dirección URL HTTPS, seleccione el **habilitar HTTP a HTTPS redirección** cuadro.  
  
    -   En el cuadro **Dirección URL del servidor back-end**, indique la dirección URL del servidor back-end. Tenga en cuenta que este valor se introduce automáticamente al escribir la dirección URL externa y debe cambiarlo solo si la dirección URL del servidor back-end es diferente; Por ejemplo, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy de aplicación Web puede traducir los nombres de host en direcciones URL, pero no los nombres de ruta. Por lo tanto, se pueden especificar otros nombres de host, pero el nombre de la ruta de acceso debe ser el mismo. Por ejemplo, puede escribir una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://app-server/app1/. Sin embargo, no se puede escribir una dirección URL externa de https://apps.contoso.com/app1/ y una dirección URL del servidor back-end de https://apps.contoso.com/internal-app1/.  
  
7.  En la página **Confirmación** , revise la configuración y, a continuación, haga clic en **Publicar**. Puede copiar el comando de PowerShell para configurar más aplicaciones publicadas.  
  
8.  En la página **Resultados**, asegúrese de que la aplicación se publicó correctamente y, a continuación, haga clic en **Cerrar**.  
  
Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Para establecer la dirección URL de autenticación de OAuth para un servidor de federación de dirección de fs.contoso.com y una ruta de acceso de dirección URL de/ADFS/oauth2 /:  
  
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
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Solución de problemas del Proxy de aplicación web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Publicar aplicaciones a través de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Planificación publicar aplicaciones mediante el Proxy de aplicación Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guía de tutorial de Proxy de aplicación Web](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Cmdlets del Proxy de aplicación Web en Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


