---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Publicación de aplicaciones con SharePoint, Exchange y RDG
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: fd706f61216ab8760d94faf98d651d17b24efc91
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447146"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Publicación de aplicaciones con SharePoint, Exchange y RDG

>Se aplica a: Windows Server 2016

**Este contenido es relevante para la versión local del Proxy de aplicación Web. Para habilitar el acceso seguro a aplicaciones locales a través de la nube, consulte el [contenido de Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  

En este tema se describe las tareas necesarias para publicar SharePoint Server, Exchange Server o puerta de enlace de escritorio remoto (RDP) a través de Proxy de aplicación Web.  

>[!NOTE]
>Esta información se proporciona como-es.  Servicios de escritorio remoto admite y se recomienda el uso de [Proxy de aplicación de Azure para proporcionar acceso remoto seguro a aplicaciones locales](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="BKMK_6.1"></a>Publicación de SharePoint Server  
Puede publicar un sitio de SharePoint a través de Proxy de aplicación Web cuando el sitio de SharePoint está configurado para la autenticación basada en notificaciones o autenticación de Windows integrada. Si desea utilizar servicios de federación de Active Directory (AD FS) para la autenticación previa, debe configurar un confianza mediante uno de los asistentes.  

-   Si el sitio de SharePoint usa la autenticación basada en notificaciones, debe utilizar el Asistente para agregar la relación de confianza para usuario autenticado para la aplicación.  

-   Si el sitio de SharePoint usa la autenticación integrada de Windows, debe utilizar el Asistente para agregar relación de confianza para usuario autenticado no basado en notificaciones para configurar la relación de confianza para usuario autenticado para la aplicación. Puede usar IWA con una aplicación web basada en notificaciones si configura el KDC.  

    Para permitir que los usuarios se autentiquen mediante la autenticación de Windows integrada, el servidor de Proxy de aplicación Web debe estar unido a un dominio.  

    Debe configurar la aplicación para que admita la delegación restringida de Kerberos. Puede hacerlo en el controlador de dominio para cualquier aplicación. También puede configurar la aplicación directamente en el servidor back-end si se está ejecutando en Windows Server 2012 R2 o Windows Server 2012. Para más información, consulte [Novedades de la autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx). También debe asegurarse de que los servidores Proxy de aplicación Web se configuran para la delegación a los nombres de entidad de servicio de los servidores back-end. Para ver un tutorial sobre cómo configurar el Proxy de aplicación Web para publicar una aplicación mediante la autenticación de Windows integrada, vea [configurar un sitio para usar la autenticación de Windows integrada](assetId:///b0788958-627f-450f-877c-209b1bd0db52).  

Si el sitio de SharePoint se configura mediante asignaciones de acceso alternativas (AAM) o colecciones de sitios denominadas host, puede usar diferentes direcciones URL de los servidores externos y back-end para publicar la aplicación. Sin embargo, si no configura el sitio de SharePoint con AAM o colecciones de sitios denominadas host, deberá usar las mismas direcciones URL de los servidores externos y back-end.  

## <a name="BKMK_6.2"></a>Publicación de Exchange Server  
En la tabla siguiente se describe los servicios de Exchange que se pueden publicar a través de Proxy de aplicación Web y la autenticación previa compatible para estos servicios:  


|    Servicio de Exchange    |                                                                            Autenticación previa                                                                            |                                                                                                                                       Notas                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS mediante autenticación no basada en notificaciones<br />: Paso a través de<br />-AD FS mediante autenticación basada en notificaciones para un entorno local Exchange 2013 Service Pack 1 (SP1) |                                                                  Para obtener más información, consulta: [Mediante la autenticación basada en notificaciones de AD FS con Outlook Web App y EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Panel de control de Exchange |                                                                               Paso a través                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook en cualquier lugar    |                                                                               Paso a través                                                                               | Debe publicar tres direcciones URL para que Outlook en cualquier lugar funcione correctamente:<br /><br />-La dirección URL de detección automática.<br />-El nombre de host externo de Exchange Server; es decir, la dirección URL que está configurada para que conectarse a los clientes.<br />-El FQDN interno del servidor de Exchange. |
|  Exchange ActiveSync   |                                                     Paso a través<br/> AD FS mediante el protocolo de autorización básica HTTP                                                      |                                                                                                                                                                                                                                                                                    |

Para publicar Outlook Web App con la autenticación integrada de Windows, debe usar el Asistente para agregar relación de confianza para usuario autenticado no basado en notificaciones para configurar la relación de confianza para la aplicación.  

Para permitir que los usuarios se autentiquen mediante Kerberos delegación restringida de que servidor Proxy de aplicación Web debe estar unido a un dominio.  

Debe configurar la aplicación para admitir la autenticación Kerberos. Además deberá registrar un nombre principal de servicio (SPN) a la cuenta que se ejecuta el servicio web. Puede hacerlo en el controlador de dominio o en los servidores back-end. En una carga equilibrada entorno Exchange esto requeriría con la cuenta de servicio alternativo, consulte [configurar la autenticación Kerberos para servidores con equilibrio de carga de acceso de cliente](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  

También puede configurar la aplicación directamente en el servidor back-end si se está ejecutando en Windows Server 2012 R2 o Windows Server 2012. Para más información, consulte [Novedades de la autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx). También debe asegurarse de que los servidores Proxy de aplicación Web se configuran para la delegación a los nombres de entidad de servicio de los servidores back-end.  

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Publicación de puerta de enlace de escritorio remoto a través de Proxy de aplicación Web  
Si desea restringir el acceso a la puerta de enlace de acceso remoto y agregar la autenticación previa para el acceso remoto, se puede desplegar a través de Proxy de aplicación Web. Se trata de una manera realmente buena para asegurarse de que tiene la autenticación previa enriquecida para RDG incluidos MFA. Publicación sin autenticación previa también es una opción y proporciona un único punto de entrada en sus sistemas.  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Cómo publicar una aplicación en RDG mediante la autenticación de paso a través del Proxy de aplicación Web  

1. Instalación será diferente dependiendo de si el acceso Web de escritorio remoto (/ rdweb) y roles de la puerta de enlace de escritorio remoto (rpc) en el mismo servidor o en servidores diferentes.  

2. Si se hospedan los roles de acceso Web de escritorio remoto y puerta de enlace de escritorio remoto en el mismo servidor RDG, simplemente se puede publicar el FQDN de raíz en el Proxy de aplicación Web, como https://rdg.contoso.com/.  

   También puede publicar los dos directorios virtuales individualmente, por ejemplo,<https://rdg.contoso.com/rdweb/> y https://rdg.contoso.com/rpc/.  

3. Si el acceso Web de escritorio remoto y la puerta de enlace de escritorio remoto se hospedan en servidores independientes de RDG, deberá publicar individualmente los dos directorios virtuales. Por ejemplo, puede usar la mismo u otro FQDN externo https://rdweb.contoso.com/rdweb/ y https://gateway.contoso.com/rpc/.  

4. Si el externo y el del FQDN interno son diferentes no debe deshabilitar la traducción de encabezado de solicitud en la regla de publicación RDWeb. Esto puede hacerse mediante la ejecución del siguiente script de PowerShell en el servidor Proxy de aplicación Web, pero se debe habilitar de forma predeterminada.

   ```  
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
   ```  

   > [!NOTE]  
   > Si tiene que admitir a clientes enriquecidos, como RemoteApp y conexiones de escritorio o conexiones de escritorio remoto de iOS, estos no admiten la autenticación previa para que tenga que publicar RDG mediante la autenticación de paso a través.  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Cómo publicar una aplicación en RDG mediante el Proxy de aplicación Web con la autenticación previa  

1.  Autenticación previa de Proxy de aplicación Web con RDG funciona pasando la cookie de autenticación previa obtenida por Internet Explorer que se pasa en el cliente conexión a Escritorio remoto (mstsc.exe). A continuación, se utiliza el cliente conexión a Escritorio remoto (mstsc.exe). A continuación, se usa el cliente conexión a Escritorio remoto como prueba de la autenticación.  

    El siguiente procedimiento indica al servidor de la colección para incluir las propiedades RDP personalizadas necesarias en los archivos RDP de aplicación remota que se envían a los clientes. Estos indican que el cliente de autenticación previa que es necesario y las cookies para la autenticación previa de pasar la dirección del servidor al cliente de conexión a Escritorio remoto (mstsc.exe). Junto con la desactivación de la función HttpOnly en la aplicación de Proxy de aplicación Web, esto permite al cliente de conexión a Escritorio remoto (mstsc.exe) usar la cookie de Proxy de aplicación Web obtenida a través del explorador.  

    Autenticación en el servidor de acceso Web de RD seguirá utilizando el inicio de sesión de formulario de acceso Web de escritorio remoto. Esto proporciona el menor número de autenticación de usuario pide como el formulario de inicio de sesión de RD Web Access crea un almacén de credenciales de cliente que, a continuación, se puede usar el cliente conexión a Escritorio remoto (mstsc.exe) para cualquier vuelva a iniciarse la aplicación remota.  

2.  En primer lugar, cree un manual de confianza en AD FS como si se publica una aplicación compatible con notificaciones. Esto significa que tendrá que crear un dummy confianza que existen exigir la autenticación previa, para que obtengan la autenticación previa sin delegación limitada de Kerberos al servidor publicado. Una vez que un usuario autenticado, todo lo demás se pasa a través.  

    > [!WARNING]  
    > Puede parecer que es preferible utilizar la delegación pero no soluciona totalmente los requisitos de inicio de sesión único de mstsc y hay problemas al delegar en el directorio /rpc porque el cliente espera que controlen la autenticación de puerta de enlace de escritorio remoto.  

3.  Para crear un manual de confianza, siga los pasos descritos en la consola de administración de AD FS:  

    1.  Use la **agregar confianza** Asistente  

    2.  Seleccione **escribir manualmente los datos sobre la confianza**.  

    3.  Acepte la configuración predeterminada.  

    4.  Para obtener el identificador de confianza, escriba el FQDN externo que usará para tener acceso RDG, por ejemplo https://rdg.contoso.com/.  

        Se trata de la relación de confianza que va a usar al publicar la aplicación en el Proxy de aplicación Web.  

4.  Publicar la raíz del sitio (por ejemplo, https://rdg.contoso.com/ ) en el Proxy de aplicación Web. Establezca la autenticación previa en AD FS y use la relación de confianza que creó anteriormente. Esto le permitirá/RDWeb y /rpc para usar la misma cookie de autenticación de Proxy de aplicación Web.  

    Es posible publicar/RDWeb y /rpc como aplicaciones independientes e incluso usar diferentes servidores publicados. Deberá asegurarse de que publica tanto con la misma confianza como el token de Proxy de aplicación Web se emite para la confianza y, por tanto, es válido en las aplicaciones publicadas con la misma confianza.  

5.  Si el externo y el del FQDN interno son diferentes no debe deshabilitar la traducción de encabezado de solicitud en la regla de publicación RDWeb. Esto puede hacerse mediante la ejecución del siguiente script de PowerShell en el servidor Proxy de aplicación Web, pero se debe habilitar de forma predeterminada:

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  

6.  Deshabilitar la propiedad de cookies HttpOnly en Proxy de aplicación Web en el RDG aplicación publicada. Para permitir el acceso de control de ActiveX RDG a la cookie de autenticación de Proxy de aplicación Web, tendrá que deshabilitar la propiedad HttpOnly en la cookie de Proxy de aplicación Web.  

    Esto requiere las siguientes acciones para instalarse [Web Application Proxy Hotfix](https://support.microsoft.com/en-gb/kb/3000850) o [ https://support.microsoft.com/en-gb/kb/3000850 ](https://support.microsoft.com/en-gb/kb/3000850).  

    Después de instalar la revisión, ejecute el siguiente script de PowerShell en el servidor Proxy de aplicación Web especificando el nombre de la aplicación en cuestión:  

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  

    Deshabilitar HttpOnly permite el acceso de control de ActiveX RDG a la cookie de autenticación de Proxy de aplicación Web.  

7.  Configurar la recopilación de RDG relevante en el servidor de recopilación para permitir que la conexión a Escritorio remoto (mstsc.exe) del cliente sabe que es necesario que la autenticación previa en el archivo rdp.  

    -   En Windows Server 2012 y Windows Server 2012 R2, esto puede realizarse mediante la ejecución del siguiente cmdlet de PowerShell en el servidor de recopilación RDG:  

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        Asegúrese de quitar el < y > corchetes cuando se reemplaza por sus propios valores, por ejemplo:

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   En Windows Server 2008 R2:  

        1.  Inicie sesión en el servidor de Terminal Server con una cuenta que tenga privilegios de administrador.  

        2.  Vaya a **iniciar** >**herramientas administrativas** > **servicios de Terminal Server** > **Administrador de RemoteApp de TS.**  

        3.  En el **Introducción** panel de TS RemoteApp Manager, junto a configuración de RDP, haga clic en **cambio**.  

        4.  En el **configuración personalizada de RDP** , escriba la siguiente configuración de RDP en el cuadro de configuración personalizada de RDP:  

            `pre-authentication server address: s: https://externalfqdn/rdweb/`  

            `require pre-authentication:i:1`  

        5.  Cuando haya terminado, haga clic en **aplicar**.  

            Esto indica al servidor de la colección para incluir las propiedades personalizadas de RDP en los archivos RDP que se envían a los clientes. Estos indican que el cliente de autenticación previa que es necesario y las cookies para la autenticación previa de pasar la dirección del servidor al cliente conexión a Escritorio remoto (mstsc.exe). Esto, junto con la desactivación HttpOnly en la aplicación de Proxy de aplicación Web, permite al cliente de conexión a Escritorio remoto (mstsc.exe) usar la cookie de autenticación de Proxy de aplicación Web obtenida a través del explorador.  

            Para obtener más información sobre RDP, consulte [configurar el escenario de OTP de puerta de enlace de TS](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx).  

## <a name="BKMK_Links"></a>Vea también  

-   [Planificación publicar aplicaciones mediante el Proxy de aplicación Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  

-   [Solución de problemas del Proxy de aplicación web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  

-   [Guía de tutorial de Proxy de aplicación Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
