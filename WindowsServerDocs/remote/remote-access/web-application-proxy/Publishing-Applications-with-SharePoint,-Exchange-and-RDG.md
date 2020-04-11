---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Publicación de aplicaciones con SharePoint, Exchange y RDG
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: f859d40ed04cc25285212968e6cd186cffe760ae
ms.sourcegitcommit: 5c93c685dca3cafeea916cedcc0f915c528484ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81119246"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Publicación de aplicaciones con SharePoint, Exchange y RDG

> Se aplica a: Windows Server 2016

**Este contenido es relevante para la versión local del proxy de aplicación Web. Para habilitar el acceso seguro a aplicaciones locales a través de la nube, consulte el [contenido del proxy de aplicación Azure ad](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**

En este tema se describen las tareas necesarias para publicar SharePoint Server, Exchange Server o la puerta de enlace de Escritorio remoto (RDP) a través del proxy de aplicación Web.

> [!NOTE]
> Esta información se proporciona tal cual.  Servicios de Escritorio remoto admite y recomienda el uso de [App de Azure proxy para proporcionar un acceso remoto seguro a aplicaciones locales](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="publish-sharepoint-server"></a><a name="BKMK_6.1"></a>Publicación de SharePoint Server
Puede publicar un sitio de SharePoint mediante el proxy de aplicación web cuando el sitio de SharePoint está configurado para la autenticación basada en notificaciones o la autenticación integrada de Windows. Si desea usar Servicios de federación de Active Directory (AD FS) (AD FS) para la autenticación previa, debe configurar un usuario de confianza mediante uno de los asistentes.

-   Si el sitio de SharePoint usa la autenticación basada en notificaciones, debe utilizar el Asistente para agregar la relación de confianza para usuario autenticado para la aplicación.

-   Si el sitio de SharePoint usa la autenticación integrada de Windows, debe utilizar el Asistente para agregar relación de confianza para usuario autenticado no basado en notificaciones para configurar la relación de confianza para usuario autenticado para la aplicación. Puede usar IWA con una aplicación web basada en notificaciones si configura el KDC.

    Para permitir que los usuarios se autentiquen mediante la autenticación integrada de Windows, el servidor proxy de aplicación Web debe estar unido a un dominio.

    Debe configurar la aplicación para que admita la delegación restringida de Kerberos. Puede hacerlo en el controlador de dominio para cualquier aplicación. También puede configurar la aplicación directamente en el servidor back-end si se está ejecutando en Windows Server 2012 R2 o en Windows Server 2012. Para obtener más información, consulta [Novedades de la autenticación de Kerberos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). También debe asegurarse de que los servidores proxy de aplicación Web estén configurados para la delegación en los nombres de entidad de seguridad de servicio de los servidores back-end. Para ver un tutorial sobre cómo configurar el proxy de aplicación web para publicar una aplicación mediante la autenticación integrada de Windows, consulte [configurar un sitio para usar la autenticación integrada de Windows](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/dn308246(v=ws.11)).

Si el sitio de SharePoint se configura mediante asignaciones de acceso alternativas (AAM) o colecciones de sitios denominadas host, puede usar diferentes direcciones URL de los servidores externos y back-end para publicar la aplicación. Sin embargo, si no configura el sitio de SharePoint con AAM o colecciones de sitios denominadas host, deberá usar las mismas direcciones URL de los servidores externos y back-end.

## <a name="publish-exchange-server"></a><a name="BKMK_6.2"></a>Publicación de Exchange Server
En la tabla siguiente se describen los servicios de Exchange que se pueden publicar a través del proxy de aplicación web y la autenticación previa admitida para estos servicios:


|    Servicio de Exchange    |                                                                            Autenticación previa                                                                            |                                                                                                                                       Notas                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS mediante la autenticación no basada en notificaciones<br />-Paso a través<br />-AD FS el uso de la autenticación basada en notificaciones para Exchange 2013 Service Pak 1 (SP1) local |                                                                  Para más información, vea: [Usar la autenticación basada en notificaciones de AD FS con Outlook Web App y EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Panel de control de Exchange |                                                                               Paso a través                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook en cualquier lugar    |                                                                               Paso a través                                                                               | Debe publicar tres direcciones URL para que Outlook en cualquier lugar funcione correctamente:<p>-La dirección URL de detección automática.<br />: El nombre de host externo del servidor de Exchange; es decir, la dirección URL que está configurada para que los clientes se conecten a.<br />: El nombre de dominio completo interno del servidor de Exchange. |
|  Exchange ActiveSync   |                                                     Paso a través<br/> AD FS mediante el protocolo de autorización básico HTTP                                                      |                                                                                                                                                                                                                                                                                    |

Para publicar Outlook Web App con la autenticación integrada de Windows, debe usar el Asistente para agregar relación de confianza para usuario autenticado no basado en notificaciones para configurar la relación de confianza para la aplicación.

Para permitir que los usuarios se autentiquen mediante la delegación limitada de Kerberos, el servidor proxy de aplicación Web debe estar unido a un dominio.

Debe configurar la aplicación para que admita la autenticación Kerberos. Además, debe registrar un nombre principal de servicio (SPN) en la cuenta en la que se está ejecutando el servicio Web. Puede hacerlo en el controlador de dominio o en los servidores back-end. En un entorno de Exchange con equilibrio de carga, esto requeriría el uso de la cuenta de servicio alternativa, consulte [configuración de la autenticación Kerberos para servidores de acceso de cliente con equilibrio de carga](https://docs.microsoft.com/exchange/configuring-kerberos-authentication-for-load-balanced-client-access-servers-exchange-2013-help)

También puede configurar la aplicación directamente en el servidor back-end si se está ejecutando en Windows Server 2012 R2 o en Windows Server 2012. Para obtener más información, consulta [Novedades de la autenticación de Kerberos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). También debe asegurarse de que los servidores proxy de aplicación Web estén configurados para la delegación en los nombres de entidad de seguridad de servicio de los servidores back-end.

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Publicación de Escritorio remoto puerta de enlace mediante el proxy de aplicación Web
Si quiere restringir el acceso a la puerta de enlace de acceso remoto y agregar la autenticación previa para el acceso remoto, puede revertirla a través del proxy de aplicación Web. Esta es una buena manera de asegurarse de que tiene una autenticación previa enriquecida para RDG, incluido MFA. La publicación sin autenticación previa también es una opción y proporciona un único punto de entrada en los sistemas.

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Publicación de una aplicación en RDG mediante la autenticación de paso a través del proxy de aplicación Web

1. La instalación será diferente en función de si los roles de acceso web de escritorio remoto (/RDWeb) y de puerta de enlace de escritorio remoto (RPC) están en el mismo servidor o en servidores diferentes.

2. Si los roles acceso web de escritorio remoto y puerta de enlace de escritorio remoto se hospedan en el mismo servidor RDG, puede simplemente publicar el FQDN raíz en el proxy de aplicación Web, como https://rdg.contoso.com/.

   También puede publicar los dos directorios virtuales individualmente, por ejemplo<https://rdg.contoso.com/rdweb/> y https://rdg.contoso.com/rpc/.

3. Si el acceso web de escritorio remoto y la puerta de enlace de escritorio remoto se hospedan en servidores independientes de RDG, tiene que publicar los dos directorios virtuales de forma individual. Puede usar el mismo nombre de dominio completo externo, por ejemplo, https://rdweb.contoso.com/rdweb/ y https://gateway.contoso.com/rpc/.

4. Si los FQDN externos e internos son diferentes, no debe deshabilitar la traducción de encabezados de solicitud en la regla de publicación de RDWeb. Esto puede hacerse mediante la ejecución del siguiente script de PowerShell en el servidor proxy de aplicación Web, pero debe estar habilitado de forma predeterminada.

   ```PowerShell
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false
   ```

   > [!NOTE]
   > Si necesita admitir clientes enriquecidos, como conexiones de RemoteApp y escritorio o conexiones de Escritorio remoto de iOS, estos no admiten la autenticación previa, por lo que tiene que publicar RDG mediante la autenticación de paso a través.

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Publicación de una aplicación en RDG con el proxy de aplicación web con autenticación previa

1.  La autenticación previa del proxy de aplicación web con RDG funciona pasando la cookie de autenticación previa obtenida por Internet Explorer que se pasa al Conexión a Escritorio remoto cliente (mstsc. exe). A continuación, el cliente de Conexión a Escritorio remoto (mstsc. exe) lo usa. Esto lo utiliza Conexión a Escritorio remoto cliente como prueba de autenticación.

    El procedimiento siguiente indica al servidor de recopilación que incluya las propiedades de RDP personalizadas necesarias en los archivos RDP de la aplicación remota que se envían a los clientes. Se indica al cliente que se requiere autenticación previa y pasar las cookies para la dirección del servidor de autenticación previa a Conexión a Escritorio remoto cliente (mstsc. exe). Junto con la deshabilitación de la característica HttpOnly en la aplicación Web Application proxy, permite que el cliente de Conexión a Escritorio remoto (mstsc. exe) Use la cookie del proxy de aplicación web obtenida a través del explorador.

    La autenticación en el servidor de acceso web de escritorio remoto seguirá usando el inicio de sesión del formulario de acceso web de escritorio remoto. Esto proporciona el menor número de solicitudes de autenticación de usuario que el formulario de inicio de sesión de acceso web de escritorio remoto crea un almacén de credenciales del lado cliente que, a continuación, Conexión a Escritorio remoto cliente (mstsc. exe) puede usar para cualquier inicio de la aplicación remota posterior.

2.  En primer lugar, cree una relación de confianza para usuario autenticado manual en AD FS como si estuviera publicando una aplicación compatible con notificaciones. Esto significa que tiene que crear una relación de confianza para usuario autenticado que esté allí para aplicar la autenticación previa, de modo que obtenga la autenticación previa sin la delegación limitada de Kerberos en el servidor publicado. Una vez que un usuario se ha autenticado, se pasa todo lo demás.

    > [!WARNING]
    > Podría parecer que el uso de la delegación es preferible, pero no soluciona totalmente los requisitos de los SSO de mstsc y hay problemas al delegar en el directorio/RPC porque el cliente espera administrar la propia autenticación de puerta de enlace de escritorio remoto.

3.  Para crear una relación de confianza para usuario autenticado manual, siga los pasos descritos en la consola de administración de AD FS:

    1.  Usar el Asistente para **Agregar relación de confianza para usuario autenticado**

    2.  Seleccione **escribir manualmente los datos sobre el usuario de confianza**.

    3.  Acepte todos los valores predeterminados.

    4.  En el caso del identificador de la relación de confianza para usuario autenticado, escriba el FQDN externo que usará para el acceso a RDG, por ejemplo https://rdg.contoso.com/.

        Esta es la relación de confianza para usuario autenticado que usará al publicar la aplicación en el proxy de aplicación Web.

4.  Publique la raíz del sitio (por ejemplo, https://rdg.contoso.com/) en el proxy de aplicación Web. Establezca la autenticación previa en AD FS y use la relación de confianza para usuario autenticado que creó anteriormente. Esto permitirá a/RDWeb y/RPC usar la misma cookie de autenticación del proxy de aplicación Web.

    Es posible publicar/RDWeb y/RPC como aplicaciones independientes e incluso usar diferentes servidores publicados. Solo tiene que asegurarse de publicar ambos usando la misma relación de confianza para usuario autenticado que el token del proxy de aplicación web se emite para la relación de confianza para usuario autenticado y, por lo tanto, es válida entre las aplicaciones publicadas con la misma relación de confianza para usuario autenticado.

5.  Si los FQDN externos e internos son diferentes, no debe deshabilitar la traducción de encabezados de solicitud en la regla de publicación de RDWeb. Esto puede hacerse mediante la ejecución del siguiente script de PowerShell en el servidor proxy de aplicación Web, pero debe estar habilitado de forma predeterminada:

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$true
    ```

6.  Deshabilite la propiedad de cookie HttpOnly en el proxy de aplicación web en la aplicación publicada RDG. Para permitir que el control ActiveX RDG tenga acceso a la cookie de autenticación del proxy de aplicación Web, tiene que deshabilitar la propiedad HttpOnly en la cookie del proxy de aplicación Web.

    Esto requiere instalar el [paquete acumulativo de actualizaciones de noviembre de 2014 para Windows RT 8,1, Windows 8.1 y Windows Server 2012 R2 (KB3000850)](https://support.microsoft.com/kb/3000850).

    Después de instalar la revisión, ejecute el siguiente script de PowerShell en el servidor proxy de aplicación web y especifique el nombre de la aplicación pertinente:

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true
    ```

    Deshabilitar HttpOnly permite que el control ActiveX RDG tenga acceso a la cookie de autenticación del proxy de aplicación Web.

7.  Configure la colección RDG relevante en el servidor de recopilación para permitir que el cliente de Conexión a Escritorio remoto (mstsc. exe) sepa que se requiere la autenticación previa en el archivo RDP.

    -   En Windows Server 2012 y Windows Server 2012 R2 esto puede realizarse mediante la ejecución del siguiente cmdlet de PowerShell en el servidor de colección RDG:

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        Asegúrese de quitar los corchetes de < y > cuando reemplace por sus propios valores, por ejemplo:

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   En Windows Server 2008 R2:

        1.  Inicie sesión en el Terminal Server con una cuenta que tenga privilegios de administrador.

        2.  Vaya a **inicio** >**herramientas administrativas** > **Terminal Services** > **Administrador de RemoteApp de TS.**

        3.  En el panel de **información general** de TS administrador de RemoteApp, junto a configuración de RDP, haga clic en **cambiar**.

        4.  En la pestaña **Configuración personalizada de RDP** , escriba la siguiente configuración de RDP en el cuadro configuración personalizada de RDP:

            `pre-authentication server address: s: https://externalfqdn/rdweb/`

            `require pre-authentication:i:1`

        5.  Cuando haya terminado, haga clic en **aplicar**.

            Esto indica al servidor de recopilación que incluya las propiedades personalizadas de RDP en los archivos RDP que se envían a los clientes. Se indica al cliente que se requiere la autenticación previa y que pase las cookies para la dirección del servidor de autenticación previa al cliente Conexión a Escritorio remoto (mstsc. exe). Esto, junto con deshabilitar HttpOnly en la aplicación de proxy de aplicación Web, permite que el cliente de Conexión a Escritorio remoto (mstsc. exe) Use la cookie de autenticación del proxy de aplicación web obtenida a través del explorador.

            Para obtener más información sobre RDP, consulte [configuración del escenario de OTP de puerta de enlace de TS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731249(v=ws.10)).

## <a name="see-also"></a><a name="BKMK_Links"></a>Vea también

- [Planeación de la publicación de aplicaciones mediante el proxy de aplicación Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))

- [Solución de problemas del Proxy de aplicación web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

- [Guía de tutorial de proxy de aplicación Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))
