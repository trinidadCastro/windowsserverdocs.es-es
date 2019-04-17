---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Directivas de Control de acceso de AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0b4549192bc4dab9edf3b2a10421325144ed0cd2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Directivas de Control de acceso de Windows Server 2012 R2 y Windows Server FS 2012 AD

>Se aplica a: Windows Server 2012 R2 y Windows Server 2012 

Hacen que las directivas descritas en este artículo usa dos tipos de notificaciones  
  
1.  Las notificaciones de que AD FS crea en función de la información de proxy de AD FS y la aplicación Web puede inspeccionar y comprobar, como la dirección IP del cliente conectarse directamente a AD FS o el WAP.  
  
2.  Reclamaciones AD FS crea en función de la información transmitida a AD FS por el cliente como los encabezados HTTP  
  
>**Importante**: las directivas que se indican a continuación se bloquear unirse a un dominio de Windows 10 e inicia sesión en escenarios que requieren acceso a los siguientes extremos adicionales

Extremos de AD FS necesarios para la unión de dominios de Windows 10 e inicia sesión en
- [nombre del servicio de federación] / adfs, servicios, confianza, 2005/windowstransport
- [nombre del servicio de federación] / adfs, servicios, confianza, 13/windowstransport
- [nombre del servicio de federación] / adfs, servicios, confianza, 2005/usernamemixed
- [nombre del servicio de federación] / adfs, servicios, confianza, 13/usernamemixed 
- [nombre del servicio de federación] / adfs, servicios, confianza, 2005/certificatemixed
- [nombre del servicio de federación] / adfs, servicios, confianza, 13/certificatemixed

Para resolver, actualizar las directivas que denegación en función de la reclamación de extremo para permitir que la excepción de los extremos anteriores.

Por ejemplo, la regla siguiente:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

Se actualizará para:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Notificaciones de esta categoría solo deben usarse para implementar las directivas de empresa y no como directivas de seguridad para proteger el acceso a la red.  Es posible que los clientes no autorizados enviar encabezados con información falsa como una forma de obtener acceso.  
  
Las directivas descritas en este artículo siempre deben usarse con otro método de autenticación, como la autenticación de factor de nombre de usuario y la contraseña o múltiple.  
  
## <a name="client-access-policies-scenarios"></a>Escenarios de directivas de acceso de cliente  
  
|**Escenario**|**Descripción**| 
| --- | --- | 
|Escenario 1: Bloquear todos los accesos externos a Office 365|Se permite el acceso de Office 365 desde todos los clientes en la red corporativa interna, pero se deniegan solicitudes de clientes externos en función de la dirección IP del cliente externo.|  
|Escenario 2: Bloquear todos los accesos externos a Office 365 excepto Exchange ActiveSync|Se permite el acceso de Office 365 desde todos los clientes en la red corporativa interna, así como de todos los dispositivos cliente externo, como teléfonos inteligentes, que utilizan Exchange ActiveSync. Todos los otros clientes externos, como los con Outlook, se bloquean.|  
|Escenario 3: Bloquear todos los accesos externos a Office 365 excepto aplicaciones basadas en explorador|Bloquea el acceso externo a Office 365, salvo para las aplicaciones pasivos (basada en explorador), como Outlook Web Access o SharePoint Online.|  
|Escenario 4: Bloquear todos los accesos externos a Office 365 excepto designados grupos de Active Directory|Este escenario se usa para probar y validar la implementación de directivas de acceso de cliente. Bloquea el acceso externo a Office 365 solo para los miembros del grupo de Active Directory de uno o más. También puede usarse para proporcionar acceso externo únicamente a los miembros de un grupo.|  
  
## <a name="enabling-client-access-policy"></a>Habilitar la directiva de acceso de cliente  
 Para habilitar la directiva de acceso de cliente de AD FS en Windows Server 2012 R2, debes actualizar la plataforma de identidad de Microsoft Office 365 confiar confianza de terceros. Elige uno de los escenarios de ejemplo siguientes para configurar las reglas de notificación en el **plataforma de identidad de Microsoft Office 365** confianza confianza de terceros que mejor satisfaga las necesidades de la organización.  
  
###  <a name="scenario1"></a>Escenario 1: Bloquear todos los accesos externos a Office 365  
 Este escenario de directiva de acceso de cliente permite el acceso de todos los clientes internos y bloques de todos los clientes externos en función de la dirección IP del cliente externo. Puedes usar los siguientes procedimientos para agregar las reglas de autorización de emisión correctas a Office 365 confiar confianza de terceros para la situación elegida.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Para crear reglas para bloquear todos los accesos externos a Office 365  
  
1.  Desde **administrador del servidor**, haz clic en **herramientas**, a continuación, haz clic en **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS\Trust relaciones**, haz clic en **confiar confía en las partes**, haz clic en el **plataforma de identidad de Microsoft Office 365** de confianza y, a continuación, haz clic en **editar reglas de notificación**.  
  
3.  En la **editar reglas de notificación** cuadro de diálogo, selecciona el **las reglas de autorización de emisión** pestaña y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla de notificación.  
  
4.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "si hay cualquier reclamación IP fuera del intervalo deseado, denegar". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de reclamación (reemplaza el valor por encima de la "x-ms-reenvía-ip del cliente" con una expresión IP válida):  
`c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la lista de las reglas de autorización de emisión antes de que el valor predeterminado **permitir acceso a todos los usuarios** regla (la regla de denegación tendrá prioridad a pesar de que aparece anteriormente en la lista).  Si no tiene el valor predeterminado acceso regla de permiso, puedes agregar uno al final de tu lista usando el lenguaje de regla de Reclamación como sigue:  </br>
    
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Para guardar las reglas nuevo, en la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar**. La lista resultante debería tener el siguiente aspecto.  
  
     ![Las reglas de autenticación de emisión](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a>Escenario 2: Bloquear todos los accesos externos a Office 365 excepto Exchange ActiveSync  
 El siguiente ejemplo permite el acceso a todas las aplicaciones de Office 365, como Exchange Online, de los clientes internos incluido Outlook. Bloquea el acceso de los clientes que residen fuera de la red corporativa, como se indica en la dirección IP del cliente, excepto para los clientes de Exchange ActiveSync, como los smartphones.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Para crear reglas para bloquear todos los accesos externos a Office 365 excepto Exchange ActiveSync  
  
1.  Desde **administrador del servidor**, haz clic en **herramientas**, a continuación, haz clic en **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS\Trust relaciones**, haz clic en **confiar confía en las partes**, haz clic en el **plataforma de identidad de Microsoft Office 365** de confianza y, a continuación, haz clic en **editar reglas de notificación**.  
  
3.  En la **editar reglas de notificación** cuadro de diálogo, selecciona el **las reglas de autorización de emisión** pestaña y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla de notificación.  
  
4.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "si hay cualquier reclamación IP fuera del intervalo deseado, emitir ipoutsiderange reclamación". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de reclamación (reemplaza el valor por encima de la "x-ms-reenvía-ip del cliente" con una expresión IP válida):  
    
    `c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la **las reglas de autorización de emisión** lista.  
  
7.  Siguiente, en la **editar reglas de notificación** cuadro de diálogo, en la **las reglas de autorización de emisión**, haga clic **Agregar regla** para iniciar el Asistente para la regla de Reclamación de nuevo.  
  
8.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
9. En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "si hay una dirección IP fuera del intervalo deseado y no hay una reclamación de aplicación-x-ms-cliente no EAS, denegar". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación:  
  

    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la **las reglas de autorización de emisión** lista.  
  
11. Siguiente, en la **editar reglas de notificación** cuadro de diálogo, en la **las reglas de autorización de emisión**, haga clic **Agregar regla** para iniciar el Asistente para la regla de Reclamación de nuevo.  
  
12. En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de reclamación,** selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
13. En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "comprobar si existe Reclamación de aplicación". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación:  
  
    ```  
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la **las reglas de autorización de emisión** lista.  
  
15. Siguiente, en la **editar reglas de notificación** cuadro de diálogo, en la **las reglas de autorización de emisión**, haga clic **Agregar regla** para iniciar el Asistente para la regla de Reclamación de nuevo.  
  
16. En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de reclamación,** selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
17. En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "denegar a los usuarios con ipoutsiderange true y la aplicación producirá un error". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación:  
  
`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Haz clic en **finalizar**. Comprueba que la nueva regla aparecerá inmediatamente debajo de la regla anterior y antes de reglas en el valor predeterminado permitir acceso a todos los usuarios en la lista de reglas de autorización de emisión (la regla de denegación tendrá prioridad a pesar de que aparece anteriormente en la lista).  </br>Si no tiene el valor predeterminado acceso regla de permiso, puedes agregar uno al final de tu lista usando el lenguaje de regla de Reclamación como sigue:</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Para guardar las reglas nuevo, en la **editar reglas de notificación** diálogo cuadro, haz clic en Aceptar. La lista resultante debería tener el siguiente aspecto.  
  
     ![Reglas de autorización de emisión](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a>Escenario 3: Bloquear todos los accesos externos a Office 365 excepto aplicaciones basadas en explorador  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para crear reglas para bloquear todos los accesos externos a Office 365 excepto aplicaciones basadas en explorador  
  
1.  Desde **administrador del servidor**, haz clic en **herramientas**, a continuación, haz clic en **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS\Trust relaciones**, haz clic en **confiar confía en las partes**, haz clic en el **plataforma de identidad de Microsoft Office 365** de confianza y, a continuación, haz clic en **editar reglas de notificación**.  
  
3.  En la **editar reglas de notificación** cuadro de diálogo, selecciona el **las reglas de autorización de emisión** pestaña y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla de notificación.  
  
4.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "si hay cualquier reclamación IP fuera del intervalo deseado, emitir ipoutsiderange reclamación". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de reclamación (reemplaza el valor por encima de la "x-ms-reenvía-ip del cliente" con una expresión IP válida):  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la **las reglas de autorización de emisión** lista.  
  
7.  Siguiente, en la **editar reglas de notificación** cuadro de diálogo, en la **las reglas de autorización de emisión**, haga clic **Agregar regla** para iniciar el Asistente para la regla de Reclamación de nuevo.  
  
8.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de reclamación,** selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
9. En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "si hay una dirección IP fuera del intervalo deseado y el extremo no es/adfs/ls, denegar". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación:  
  
 
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la lista de las reglas de autorización de emisión antes de que el valor predeterminado **permitir acceso a todos los usuarios** regla (la regla de denegación tendrá prioridad a pesar de que aparece anteriormente en la lista).  </br></br> Si no tiene el valor predeterminado acceso regla de permiso, puedes agregar uno al final de tu lista usando el lenguaje de regla de Reclamación como sigue:  
  
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. Para guardar las reglas nuevo, en la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar**. La lista resultante debería tener el siguiente aspecto.  
  
     ![Emisión](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a>Escenario 4: Bloquear todos los accesos externos a Office 365 excepto designados grupos de Active Directory  
 El siguiente ejemplo permite el acceso de los clientes internos en función de la dirección IP. Bloquea el acceso de los clientes que residen fuera de la red corporativa que tienen una dirección IP de cliente externo, excepto las personas de un especificado de Active Directory Group.Use estos pasos para agregar las reglas de autorización de emisión correctas para la **plataforma de identidad de Microsoft Office 365** confianza confianza de terceros con el Asistente para la regla de notificación:  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Para crear reglas para bloquear todos los accesos externos a Office 365, excepto para designado grupos de Active Directory  
  
1.  Desde **administrador del servidor**, haz clic en **herramientas**, a continuación, haz clic en **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS\Trust relaciones**, haz clic en **confiar confía en las partes**, haz clic en el **plataforma de identidad de Microsoft Office 365** de confianza y, a continuación, haz clic en **editar reglas de notificación**.  
  
3.  En la **editar reglas de notificación** cuadro de diálogo, selecciona el **las reglas de autorización de emisión** pestaña y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla de notificación.  
  
4.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "si hay cualquier reclamación IP fuera del intervalo deseado, emitir ipoutsiderange reclamación." En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de reclamación (reemplaza el valor por encima de la "x-ms-reenvía-ip del cliente" con una expresión IP válida):  
  
      
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la **las reglas de autorización de emisión** lista.  
  
7.  Siguiente, en la **editar reglas de notificación** cuadro de diálogo, en la **las reglas de autorización de emisión**, haga clic **Agregar regla** para iniciar el Asistente para la regla de Reclamación de nuevo.  
  
8.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de reclamación,** selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
9. En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "comprobar el SID del grupo". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de reclamación (reemplazo "SIDDeGrupo" con el SID del grupo de anuncios que estás usando real):  
   
    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Haz clic en **finalizar**. Comprueba que la nueva regla aparece en la **las reglas de autorización de emisión** lista.  
  
11. Siguiente, en la **editar reglas de notificación** cuadro de diálogo, en la **las reglas de autorización de emisión**, haga clic **Agregar regla** para iniciar el Asistente para la regla de Reclamación de nuevo.  
  
12. En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de reclamación,** selecciona **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
13. En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla, por ejemplo "denegar a los usuarios con ipoutsiderange true y SIDDeGrupo producirá un error". En **regla personalizada**, escribe o pega la siguiente sintaxis de lenguaje de regla de notificación:  
   
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Haz clic en **finalizar**. Comprueba que la nueva regla aparecerá inmediatamente debajo de la regla anterior y antes de reglas en el valor predeterminado permitir acceso a todos los usuarios en la lista de reglas de autorización de emisión (la regla de denegación tendrá prioridad a pesar de que aparece anteriormente en la lista).  </br></br>Si no tiene el valor predeterminado acceso regla de permiso, puedes agregar uno al final de tu lista usando el lenguaje de regla de Reclamación como sigue:  
   
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Para guardar las reglas nuevo, en la **editar reglas de notificación** diálogo cuadro, haz clic en Aceptar. La lista resultante debería tener el siguiente aspecto.  
  
     ![Emisión](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a>Creación de la expresión del intervalo de direcciones IP  
 La notificación de x-ms-reenvía-ip del cliente se rellena con un encabezado HTTP que está establecido actualmente solo por Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación de AD FS. El valor de la notificación puede ser uno de los siguientes:  
  
> [!NOTE]
>  Exchange Online solo las direcciones IPV4 e IPV6 no admite actualmente.  
  
-   Una sola dirección IP: la dirección IP del cliente que está conectada directamente al Exchange Online  
  
> [!NOTE]
>  -   La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa del servidor proxy de salida de la organización o puerta de enlace.  
> -   Los clientes que se conectan a la red corporativa a una VPN o por Microsoft DirectAccess (DA) pueden aparecer como clientes corporativos internos o externos clientes según la configuración de VPN o DA.  
  
-   Una o más direcciones IP: cuando Exchange en línea no puede determinar la dirección IP del cliente, establezca el valor en función del valor del x reenvía de encabezado, un encabezado estándar que se pueden incluir en las solicitudes HTTP y es compatible con muchos de los clientes, equilibradores de carga y servidores proxy en el mercado.  
  
> [!NOTE]
>  1.  Varias direcciones IP, que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud, se le separadas por comas.  
> 2.  Direcciones IP relacionadas con la infraestructura de Exchange Online no funcionará en la lista.  
  
### <a name="regular-expressions"></a>Expresiones regulares  
 Una vez para que coincida con un intervalo de direcciones IP, es necesario construir una expresión regular para realizar la comparación. En la siguiente serie de pasos, se proporciona ejemplos de cómo crear este tipo de expresión para que coincida con los siguientes intervalos de direcciones (Ten en cuenta que tendrás que cambiar estos ejemplos para que coincida con el intervalo de IP público):  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 En primer lugar, el patrón básico que coinciden con una sola dirección IP es la siguiente: \b###\\.###\\.###\\.###\b  
  
 Extender esto, podemos encontrar dos direcciones IP diferentes con una expresión o como sigue: \b###\\.###\\.###\\.###\b & #124;\b###\\.###\\.###\\.###\b  
  
 Por este motivo, sería un ejemplo para que coincida con tan solo dos direcciones (por ejemplo, 192.168.1.1 o 10.0.0.1): \b192\\.168\\.1\\.1\b & #124;\b10\\.0\\.0\\.1\b  
  
 Esto le da la técnica que puede especificar cualquier número de direcciones. Cuando se necesita un intervalo de direcciones poder, por ejemplo 192.168.1.1 – 192.168.1.25, establecer la coincidencia debe realizarse carácter por carácter: \b192\\.168\\.1\\. ([1-9] & #124; 1 [0-9] & #124; 2 [0-5]) \b  
  
 Ten en cuenta lo siguiente:  
  
-   La dirección IP se trata como la cadena y no es un número.  
  
-   La regla se desglosa como sigue: \b192\\.168\\.1\\.  
  
-   Esto coincide con cualquier valor que empieza por 192.168.1.  
  
-   El siguiente coincide con los intervalos necesarios para la parte de la dirección después del último punto decimal:  
  
    -   ([1-9] coincide con las direcciones que terminan en 1-9  
  
    -   & #124; 1 [0-9] coincide con direcciones que terminen en 10 19  
  
    -   & #124;2[0-5]) direcciones de coincidencias que terminen en 20-25  
  
-   Ten en cuenta que se deben colocar correctamente los paréntesis, para que no se inician coincidencia otras partes de direcciones IP.  
  
-   Con el bloque 192 coinciden, podemos escribir una expresión similar para el bloque de 10: \b10\\.0\\.0\\. ([1-9] & #124; 1 [0-4]) \b  
  
-   Y agrupamiento, la siguiente expresión debe coincidir con todas las direcciones de "192.168.1.1~25" y "10.0.0.1~14": \b192\\.168\\.1\\. ([1-9] & #124; 1 [0-9] & #124; 2 [0-5]) \b & #124;\b10\\.0\\.0\\. ([1-9] & #124; 1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>Pruebas de la expresión  
 Regex expresiones pueden tornarse muy complicadas, por lo que es muy recomendable mediante una herramienta de comprobación de la expresión regular. Si haces una búsqueda de "generador de expresiones regex en línea" internet, encontrarás varias utilidades buena en línea que te permitirá probar las expresiones de los datos de ejemplo.  
  
 Al probar la expresión, es importante comprenderlo que esperar a deben coincidir. El sistema de Exchange en línea puede enviar varias direcciones IP separadas por comas. Las expresiones proporcionadas anteriormente funcionará para esto. Sin embargo, es importante pensar esto cuando pruebes las expresiones de expresión regular. Por ejemplo, uno podría usar la siguiente muestra de entrada para comprobar los ejemplos anteriores:  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>Los tipos de notificación  
 AD FS en Windows Server 2012 R2, proporciona información de contexto de solicitud con los siguientes tipos de notificación:  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-reenvía-IP del cliente  
 Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 Esta notificación de AD FS representa un "mejor intento" determinar la dirección IP del usuario (por ejemplo, el cliente de Outlook) que realiza la solicitud. Esta notificación puede contener varias direcciones IP, incluida la dirección de cada proxy que reenvía la solicitud.  Esta notificación se rellena con un HTTP. El valor de la notificación puede ser una de las siguientes acciones:  
  
-   Una sola dirección IP: la dirección IP del cliente que está conectada directamente al Exchange Online  
  
> [!NOTE]
>  La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa del servidor proxy de salida de la organización o puerta de enlace.  
  
-   Una o varias direcciones IP  
  
    -   Si no puede determinar la dirección IP del cliente Exchange Online, establecerá el valor en función del valor de encabezado x reenvía para solicitudes de un encabezado estándar que puede incluirse en basados en HTTP y es compatible con muchos de los clientes, equilibradores de carga y servidores proxy en el mercado.  
  
    -   Varias direcciones IP que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud se se separados por comas.  
  
> [!NOTE]
>  Direcciones IP relacionadas con la infraestructura de Exchange Online no estarán presentes en la lista.  
  
> [!WARNING]
>  Exchange Online actualmente admite solo las direcciones IPV4; no admite las direcciones IPV6.  
  
### <a name="x-ms-client-application"></a>X-MS-aplicación de cliente  
 Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 Esta notificación de AD FS representa el protocolo usado por el cliente final, que corresponde ligeramente a la aplicación usada.  Esta notificación se rellena con un encabezado HTTP que se encuentra actualmente solo se establece mediante Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación de AD FS. Dependiendo de la aplicación, el valor de esta notificación será uno de los siguientes:  
  
-   En el caso de dispositivos que usan Exchange Active Sync, el valor es Microsoft.Exchange.ActiveSync.  
  
-   Uso del cliente de Microsoft Outlook podría resultar en cualquiera de los siguientes valores:  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   Otros valores posibles para este encabezado incluyen lo siguiente:  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS-cliente de agente de usuario  
 Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 Esta notificación de AD FS proporciona una cadena que represente el tipo de dispositivo que el cliente está usando para acceder al servicio. Esto se puede usar cuando los clientes le gustaría impedir el acceso para ciertos dispositivos (por ejemplo, determinados tipos de teléfonos inteligentes).  Ejemplos de valores para esta notificación incluyen (pero no se limitan a) los siguientes valores.  
  
 El a continuación se muestran algunos ejemplos de lo que puede contener el valor de x-ms-agente de usuario para un cliente cuya x-ms-aplicación de cliente es "Microsoft.Exchange.ActiveSync"  
  
-   Vórtice/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0.3  
  
 También es posible que este valor está vacío.  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 Esta notificación de AD FS indica que la solicitud se ha pasado a través del proxy de aplicación Web.  Esta notificación se rellena mediante el proxy de aplicación Web, que rellena el encabezado al pasar el back-end de servicios de federación de la solicitud de autenticación. AD FS después convierte en una reclamación.  
  
 El valor de la notificación es el nombre DNS de lo proxy de aplicación Web que pasa la solicitud.  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo de notificación: `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 Tipo de notificación similar a la anterior x-ms-proxy, este tipo de notificación indica si la solicitud se ha pasado a través del proxy de aplicación web. A diferencia de x-ms-proxy insidecorporatenetwork es un valor booleano True que indica una solicitud directamente al servicio de federación desde dentro de la red corporativa.  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-extremo-absoluta-ruta de acceso (Active vs pasivo)  
 Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 Este tipo de notificación puede usarse para determinar las solicitudes procedentes de los clientes (enriquecidos) "activos" frente a los clientes "pasivos" (explorador basado en web). Esto permite que las solicitudes externas desde aplicaciones basadas en el explorador, como Outlook Web Access, SharePoint Online o el portal de Office 365 para poder mientras se bloquean las solicitudes procedentes de los clientes enriquecidos, como Microsoft Outlook.  
  
 El valor de la notificación es el nombre del servicio de AD FS que recibe la solicitud.  
  
## <a name="see-also"></a>Consulta también  
 [AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)
