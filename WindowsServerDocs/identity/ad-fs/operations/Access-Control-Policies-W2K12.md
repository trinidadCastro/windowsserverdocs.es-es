---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Directivas de Control de acceso en AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7ae66fd47953017652ed1e753279e344e0a6c478
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949410"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Access Control directivas en Windows Server 2012 R2 y Windows Server 2012 AD FS


Las directivas descritas en este artículo hacen uso de dos tipos de notificaciones  

1.  Las notificaciones AD FS crean basándose en la información que los AD FS y el proxy de aplicación Web pueden inspeccionar y comprobar, como la dirección IP del cliente que se conecta directamente a AD FS o a WAP.  

2.  Las notificaciones AD FS crean en función de la información reenviada a AD FS por el cliente como encabezados HTTP  

>**Importante**: las directivas que se describen a continuación bloquearán escenarios de inicio de sesión y de unión a un dominio de Windows 10 que requieran acceso a los siguientes puntos de conexión adicionales.

AD FS puntos de conexión necesarios para la Unión a un dominio de Windows 10 y el inicio de sesión
- [nombre del servicio de Federación]/ADFS/Services/Trust/2005/windowstransport
- [nombre del servicio de Federación]/ADFS/Services/Trust/13/windowstransport
- [nombre del servicio de Federación]/ADFS/Services/Trust/2005/usernamemixed
- [nombre del servicio de Federación]/ADFS/Services/Trust/13/usernamemixed 
- [nombre del servicio de Federación]/ADFS/Services/Trust/2005/certificatemixed
- [nombre del servicio de Federación]/ADFS/Services/Trust/13/certificatemixed

>**Importante**: los extremos/ADFS/Services/Trust/2005/windowstransport y/ADFS/Services/Trust/13/windowstransport solo se deben habilitar para el acceso a la intranet, ya que están diseñados para ser puntos de conexión orientados a la intranet que usan el enlace WIA en https. Exponerlos a la extranet podría permitir que las solicitudes a estos puntos de conexión omitan las protecciones de bloqueo. Estos extremos deben deshabilitarse en el proxy (es decir, deshabilitarse de la extranet) para proteger el bloqueo de cuentas de AD. 

Para resolverlo, actualice las directivas que deniegan en función de la demanda del punto de conexión para permitir la excepción para los puntos de conexión anteriores.

Por ejemplo, la regla siguiente:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

se actualizaría a:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Las notificaciones de esta categoría solo deben usarse para implementar directivas empresariales y no como directivas de seguridad para proteger el acceso a la red.  Es posible que los clientes no autorizados envíen encabezados con información falsa como una manera de obtener acceso.  

Las directivas descritas en este artículo deben usarse siempre con otro método de autenticación, como el nombre de usuario y la contraseña o la autenticación multifactor.  

## <a name="client-access-policies-scenarios"></a>Escenarios de directivas de acceso de cliente  

|**Escenario**|**Descripción**| 
| --- | --- | 
|Escenario 1: bloquear todo el acceso externo a Office 365|Se permite el acceso a Office 365 desde todos los clientes de la red corporativa interna, pero las solicitudes de los clientes externos se deniegan en función de la dirección IP del cliente externo.|  
|Escenario 2: bloquear todo el acceso externo a Office 365 excepto Exchange ActiveSync|Se permite el acceso a Office 365 desde todos los clientes de la red corporativa interna, así como desde cualquier dispositivo cliente externo, como smartphones, que utilizan Exchange ActiveSync. Todos los demás clientes externos, como los que usan Outlook, están bloqueados.|  
|Escenario 3: bloquear todo el acceso externo a Office 365 excepto las aplicaciones basadas en explorador|Bloquea el acceso externo a Office 365, excepto para las aplicaciones pasivas (basadas en explorador) como Outlook Web Access o SharePoint Online.|  
|Escenario 4: bloquear todo el acceso externo a Office 365 excepto los grupos de Active Directory designados|Este escenario se usa para probar y validar la implementación de directivas de acceso de cliente. Bloquea el acceso externo a Office 365 solo para miembros de uno o más grupos de Active Directory. También se puede usar para proporcionar acceso externo solo a los miembros de un grupo.|  

## <a name="enabling-client-access-policy"></a>Habilitar la Directiva de acceso de cliente  
 Para habilitar la Directiva de acceso de cliente en AD FS en Windows Server 2012 R2, debe actualizar la relación de confianza para usuario autenticado de Microsoft Office 365 de la plataforma de identidad. Elige uno de los siguientes escenarios de ejemplo para configurar las reglas de notificaciones en la **Microsoft Office 365** confianza del usuario de confianza de la plataforma de identidad que mejor se adapte a las necesidades de tu organización.  

###  <a name="scenario1"></a>Escenario 1: bloquear todo el acceso externo a Office 365  
 Este escenario de directiva de acceso de cliente permite el acceso desde todos los clientes internos y bloquea todos los clientes externos en función de la dirección IP del cliente externo. Puede usar los procedimientos siguientes para agregar las reglas de autorización de emisión correctas a la relación de confianza para usuario autenticado de Office 365 para el escenario elegido.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Para crear reglas para bloquear todo el acceso externo a Office 365  

1.  En **Administrador del servidor**, haga clic en **herramientas**y, a continuación, en **Administración de AD FS**.  

2.  En el árbol de consola, en **relaciones de ad fs\relaciones**, haga clic en relaciones de confianza para usuario **autenticado**, haga clic con el botón secundario en la **Microsoft Office 365 Identity Platform** Trust y, a continuación, haga clic en **editar reglas de notificaciones**.  

3.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione la pestaña **reglas de autorización de emisión** y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas de notificaciones.  

4.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "si hay alguna demanda IP fuera del intervalo deseado, denegar". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones (Reemplace el valor anterior para "x-MS-forwarded-Client-IP" por una expresión IP válida):  
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista reglas de autorización de emisión antes de la regla **permitir el acceso predeterminado a todos los usuarios** (la regla de denegación tendrá prioridad aunque aparezca antes en la lista).  Si no tiene la regla de permiso de acceso predeterminada, puede Agregar una al final de la lista mediante el lenguaje de reglas de notificaciones como se indica a continuación:  </br>

    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Para guardar las nuevas reglas, en el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar**. La lista resultante debería tener un aspecto similar al siguiente.  

     ![Reglas de autenticación de emisión](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario2"></a>Escenario 2: bloquear todo el acceso externo a Office 365 excepto Exchange ActiveSync  
 En el ejemplo siguiente se permite el acceso a todas las aplicaciones de Office 365, incluido Exchange Online, desde clientes internos, incluido Outlook. Bloquea el acceso desde los clientes que se encuentran fuera de la red corporativa, como indica la dirección IP del cliente, excepto en el caso de los clientes de Exchange ActiveSync, como smartphones.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Para crear reglas para bloquear todo el acceso externo a Office 365 excepto Exchange ActiveSync  

1.  En **Administrador del servidor**, haga clic en **herramientas**y, a continuación, en **Administración de AD FS**.  

2.  En el árbol de consola, en **relaciones de ad fs\relaciones**, haga clic en relaciones de confianza para usuario **autenticado**, haga clic con el botón secundario en la **Microsoft Office 365 Identity Platform** Trust y, a continuación, haga clic en **editar reglas de notificaciones**.  

3.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione la pestaña **reglas de autorización de emisión** y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas de notificaciones.  

4.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "si hay alguna Claim IP fuera del intervalo deseado, emita ipoutsiderange claim". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones (Reemplace el valor anterior para "x-MS-forwarded-Client-IP" por una expresión IP válida):  

    `c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista **reglas de autorización de emisión** .  

7.  A continuación, en el cuadro de diálogo **editar reglas de notificaciones** , en la pestaña **reglas de autorización de emisión** , haga clic en **Agregar regla** para iniciar de nuevo el Asistente para reglas de notificaciones.  

8.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

9. En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "si hay una dirección IP fuera del intervalo deseado y hay una solicitud de aplicación, denegar, no EAS x-MS-Client. En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista **reglas de autorización de emisión** .  

11. A continuación, en el cuadro de diálogo **editar reglas de notificaciones** , en la pestaña **reglas de autorización de emisión** , haga clic en **Agregar regla** para iniciar de nuevo el Asistente para reglas de notificaciones.  

12. En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

13. En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo "comprobar si existe la solicitud de aplicación". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones:  

   ```  
   NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista **reglas de autorización de emisión** .  

15. A continuación, en el cuadro de diálogo **editar reglas de notificaciones** , en la pestaña **reglas de autorización de emisión** , haga clic en **Agregar regla** para iniciar de nuevo el Asistente para reglas de notificaciones.  

16. En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

17. En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo "denegar a los usuarios con ipoutsiderange true y error de aplicación". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones:  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece inmediatamente debajo de la regla anterior y antes de la regla permitir el acceso predeterminado a todos los usuarios de la lista de reglas de autorización de emisión (la regla de denegación tendrá prioridad aunque aparezca antes en la lista).  </br>Si no tiene la regla de permiso de acceso predeterminada, puede Agregar una al final de la lista mediante el lenguaje de reglas de notificaciones como se indica a continuación:</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Para guardar las nuevas reglas, en el cuadro de diálogo **editar reglas de notificaciones** , haga clic en Aceptar. La lista resultante debería tener un aspecto similar al siguiente.  

    ![Reglas de autorización de emisión](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario3"></a>Escenario 3: bloquear todo el acceso externo a Office 365 excepto las aplicaciones basadas en explorador  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para crear reglas para bloquear todo el acceso externo a Office 365 excepto las aplicaciones basadas en explorador  

1.  En **Administrador del servidor**, haga clic en **herramientas**y, a continuación, en **Administración de AD FS**.  

2.  En el árbol de consola, en **relaciones de ad fs\relaciones**, haga clic en relaciones de confianza para usuario **autenticado**, haga clic con el botón secundario en la **Microsoft Office 365 Identity Platform** Trust y, a continuación, haga clic en **editar reglas de notificaciones**.  

3.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione la pestaña **reglas de autorización de emisión** y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas de notificaciones.  

4.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "si hay alguna Claim IP fuera del intervalo deseado, emita ipoutsiderange claim". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones (Reemplace el valor anterior para "x-MS-forwarded-Client-IP" por una expresión IP válida):  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista **reglas de autorización de emisión** .  

7.  A continuación, en el cuadro de diálogo **editar reglas de notificaciones** , en la pestaña **reglas de autorización de emisión** , haga clic en **Agregar regla** para iniciar de nuevo el Asistente para reglas de notificaciones.  

8.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

9. En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "si hay una IP fuera del intervalo deseado y el extremo no es/ADFS/LS, deny". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista reglas de autorización de emisión antes de la regla **permitir el acceso predeterminado a todos los usuarios** (la regla de denegación tendrá prioridad aunque aparezca antes en la lista).  </br></br> Si no tiene la regla de permiso de acceso predeterminada, puede Agregar una al final de la lista mediante el lenguaje de reglas de notificaciones como se indica a continuación:  

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Para guardar las nuevas reglas, en el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar**. La lista resultante debería tener un aspecto similar al siguiente.  

    ![Emisión](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario4"></a>Escenario 4: bloquear todo el acceso externo a Office 365 excepto los grupos de Active Directory designados  
 En el ejemplo siguiente se habilita el acceso desde clientes internos basados en la dirección IP. Bloquea el acceso desde los clientes que se encuentran fuera de la red corporativa que tienen una dirección IP de cliente externa, excepto para las personas de un grupo de Active Directory especificado. Use los pasos siguientes para agregar las reglas de autorización de emisión correctas a la relación de confianza para usuario autenticado de la **plataforma de identidad Microsoft Office 365** mediante el Asistente para reglas de notificaciones:  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Para crear reglas para bloquear todo el acceso externo a Office 365, excepto los grupos de Active Directory designados  

1.  En **Administrador del servidor**, haga clic en **herramientas**y, a continuación, en **Administración de AD FS**.  

2.  En el árbol de consola, en **relaciones de ad fs\relaciones**, haga clic en relaciones de confianza para usuario **autenticado**, haga clic con el botón secundario en la **Microsoft Office 365 Identity Platform** Trust y, a continuación, haga clic en **editar reglas de notificaciones**.  

3.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione la pestaña **reglas de autorización de emisión** y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas de notificaciones.  

4.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "si hay alguna Claim IP fuera del intervalo deseado, emita ipoutsiderange claim". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones (Reemplace el valor anterior para "x-MS-forwarded-Client-IP" por una expresión IP válida):  


~~~
`c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista **reglas de autorización de emisión** .  

7. A continuación, en el cuadro de diálogo **editar reglas de notificaciones** , en la pestaña **reglas de autorización de emisión** , haga clic en **Agregar regla** para iniciar de nuevo el Asistente para reglas de notificaciones.  

8. En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

9. En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo "comprobar SID de grupo". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones (reemplace "GroupSID" por el SID real del grupo de ad que está usando):  

    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista **reglas de autorización de emisión** .  

11. A continuación, en el cuadro de diálogo **editar reglas de notificaciones** , en la pestaña **reglas de autorización de emisión** , haga clic en **Agregar regla** para iniciar de nuevo el Asistente para reglas de notificaciones.  

12. En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante una regla personalizada**y, a continuación, haga clic en **siguiente**.  

13. En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla, por ejemplo, "denegar a los usuarios con ipoutsiderange true y GroupSID FAIL". En **regla personalizada**, escriba o pegue la siguiente sintaxis del lenguaje de reglas de notificaciones:  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Haz clic en **Finalizar**. Compruebe que la nueva regla aparece inmediatamente debajo de la regla anterior y antes de la regla permitir el acceso predeterminado a todos los usuarios de la lista de reglas de autorización de emisión (la regla de denegación tendrá prioridad aunque aparezca antes en la lista).  </br></br>Si no tiene la regla de permiso de acceso predeterminada, puede Agregar una al final de la lista mediante el lenguaje de reglas de notificaciones como se indica a continuación:  

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Para guardar las nuevas reglas, en el cuadro de diálogo **editar reglas de notificaciones** , haga clic en Aceptar. La lista resultante debería tener un aspecto similar al siguiente.  

     ![Emisión](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="buildingip"></a>Compilar la expresión de intervalo de direcciones IP  
 La solicitud x-MS-forwarded-Client-IP se rellena a partir de un encabezado HTTP que está establecido actualmente en Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación a AD FS. El valor de la demanda puede ser uno de los siguientes:  

> [!NOTE]
>  Actualmente, Exchange Online solo admite direcciones IPV4 y no IPV6.  

-   Una única dirección IP: la dirección IP del cliente que está conectada directamente a Exchange Online  

> [!NOTE]
> - La dirección IP de un cliente de la red corporativa aparecerá como la dirección IP de la interfaz externa del proxy o la puerta de enlace de salida de la organización.  
>   -   Los clientes que están conectados a la red corporativa mediante una VPN o Microsoft DirectAccess (DA) pueden aparecer como clientes corporativos internos o como clientes externos, en función de la configuración de VPN o DA.  

-   Una o varias direcciones IP: cuando Exchange online no puede determinar la dirección IP del cliente que se conecta, establecerá el valor según el valor del encabezado x-forwarded-for, un encabezado no estándar que se puede incluir en las solicitudes basadas en HTTP y es compatible con muchos clientes, equilibradores de carga y servidores proxy en el mercado.  

> [!NOTE]
> 1. Varias direcciones IP, que indican la dirección IP del cliente y la dirección de cada proxy que pasó la solicitud, se separan con una coma.  
>    2. Las direcciones IP relacionadas con la infraestructura de Exchange online no se mostrarán en la lista.  

### <a name="regular-expressions"></a>Expresiones regulares  
 Cuando tiene que coincidir con un intervalo de direcciones IP, es necesario construir una expresión regular para realizar la comparación. En la siguiente serie de pasos, proporcionaremos ejemplos sobre cómo construir una expresión de este tipo para que coincida con los intervalos de direcciones siguientes (tenga en cuenta que tendrá que cambiar estos ejemplos para que coincidan con el intervalo de IP pública):  

- 192.168.1.1 – 192.168.1.25  

- 10.0.0.1:10.0.0.14  

  En primer lugar, el patrón básico que coincidirá con una sola dirección IP es el siguiente: \b # # #\\. # # #\\. # # #\\. # # # \b  

  Extendiendo esto, podemos hacer coincidir dos direcciones IP diferentes con una expresión OR como se indica a continuación: \b # # #\\. # # #\\. # # #\\&#124;. # # # \b \b # # #\\. # # #\\. # # #\\. # # # \b  

  Por lo tanto, un ejemplo para hacer coincidir solo dos direcciones (por ejemplo, 192.168.1.1 o 10.0.0.1) sería: \b192\\. 168\\,1&#124;\\,1 \ b \b10\\,0\\. 0\\. 1 \ b  

  Esto le ofrece la técnica por la que puede escribir cualquier número de direcciones. En el caso de que sea necesario permitir un intervalo de direcciones, por ejemplo 192.168.1.1 – 192.168.1.25, la coincidencia se debe hacer carácter por carácter: \b192\\. 168\\. 1\\. ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b  

  Tenga en cuenta lo siguiente:  

- La dirección IP se trata como una cadena y no como un número.  

- La regla se desglosa de la siguiente manera: \b192\\. 168\\. 1\\.  

- Coincide con cualquier valor que comience por 192.168.1.  

- El siguiente valor coincide con los intervalos necesarios para la parte de la dirección después del último separador decimal:  

  -   ([1-9] coincide con las direcciones que terminan en 1-9  

  -   &#124;1 [0-9] coincide con las direcciones que terminan en 10-19  

  -   &#124;2 [0-5]) coincide con las direcciones que terminan en 20-25  

- Tenga en cuenta que los paréntesis deben estar colocados correctamente, de modo que no empiece a buscar coincidencias con otras partes de direcciones IP.  

- Con el bloque 192 coincidente, podemos escribir una expresión similar para el bloque 10: \b10\\,0\\. 0\\. ([1-9]&#124;1 [0-4]) \b  

- Además de colocarlas juntas, la siguiente expresión debe coincidir con todas las direcciones de "192.168.1.1 ~ 25" y "10.0.0.1 ~ 14": \b192\\. 168\\. 1\\. ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\,0\\. 0\\. ([1-9]&#124;1 [0-4]) \b  

### <a name="testing-the-expression"></a>Probar la expresión  
 Las expresiones regex pueden resultar bastante complicadas, por lo que se recomienda encarecidamente usar una herramienta de comprobación de Regex. Si realiza una búsqueda en Internet de "generador de expresiones regex en línea", encontrará varias utilidades en línea adecuadas que le permitirán probar sus expresiones con datos de ejemplo.  

 Al probar la expresión, es importante que comprenda qué se espera que coincida. El sistema de Exchange Online puede enviar muchas direcciones IP, separadas por comas. Las expresiones proporcionadas anteriormente funcionarán para este. Sin embargo, es importante pensar en esto al probar las expresiones Regex. Por ejemplo, puede usar la siguiente entrada de ejemplo para comprobar los ejemplos anteriores:  

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1  

## <a name="claim-types"></a>Tipos de notificación  
 AD FS en Windows Server 2012 R2 proporciona información de contexto de la solicitud con los siguientes tipos de notificaciones:  

### <a name="x-ms-forwarded-client-ip"></a>X-MS-forwarded-Client-IP  
 Tipo de Claim: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 Esta demanda AD FS representa un "mejor intento" en la dirección IP del usuario (por ejemplo, el cliente de Outlook) que realiza la solicitud. Esta solicitud puede contener varias direcciones IP, incluida la dirección de cada proxy que reenvió la solicitud.  Esta demanda se rellena desde un HTTP. El valor de la demanda puede ser uno de los siguientes:  

-   Una dirección IP única: la dirección IP del cliente que está conectada directamente a Exchange Online.  

> [!NOTE]
>  La dirección IP de un cliente de la red corporativa aparecerá como la dirección IP de la interfaz externa del proxy o la puerta de enlace de salida de la organización.  

-   Una o varias direcciones IP  

    -   Si Exchange online no puede determinar la dirección IP del cliente que se conecta, establecerá el valor según el valor del encabezado x-forwarded-for, un encabezado no estándar que se puede incluir en las solicitudes basadas en HTTP y es compatible con muchos clientes, equilibradores de carga y servidores proxy en el mercado.  

    -   Varias direcciones IP que indican la dirección IP del cliente y la dirección de cada proxy que pasó la solicitud se separan con una coma.  

> [!NOTE]
>  Las direcciones IP relacionadas con la infraestructura de Exchange online no estarán presentes en la lista.  

> [!WARNING]
>  Actualmente, Exchange Online solo admite direcciones IPV4; no es compatible con las direcciones IPV6.  

### <a name="x-ms-client-application"></a>X-MS-Client-Application  
 Tipo de Claim: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 Esta demanda AD FS representa el protocolo usado por el cliente final, que se corresponde de forma flexible a la aplicación que se está usando.  Esta solicitud se rellena a partir de un encabezado HTTP que actualmente solo está establecido en Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación a AD FS. En función de la aplicación, el valor de esta demanda será uno de los siguientes:  

-   En el caso de los dispositivos que usan Exchange Active Sync, el valor es Microsoft. Exchange. ActiveSync.  

-   El uso del cliente de Microsoft Outlook puede dar lugar a cualquiera de los siguientes valores:  

    -   Microsoft. Exchange. Autodiscover  

    -   Microsoft. Exchange. OfflineAddressBook  

    -   Microsoft. Exchange. RPCMicrosoft. Exchange. webservices  

    -   Microsoft. Exchange. RPCMicrosoft. Exchange. webservices  

-   Otros valores posibles para este encabezado son los siguientes:  

    -   Microsoft. Exchange. PowerShell  

    -   Microsoft. Exchange. SMTP  

    -   Microsoft. Exchange. pop  

    -   Microsoft. Exchange. IMAP  

### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 Tipo de Claim: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 Esta AD FS Claim proporciona una cadena que representa el tipo de dispositivo que el cliente está usando para obtener acceso al servicio. Se puede usar cuando los clientes deseen evitar el acceso a determinados dispositivos (por ejemplo, tipos particulares de teléfonos inteligentes).  Los valores de ejemplo de esta demanda incluyen (pero no se limitan a) los valores siguientes.  

 A continuación se muestran ejemplos de lo que puede contener el valor x-ms-User-Agent para un cliente cuya x-MS-Client-Application es "Microsoft. Exchange. ActiveSync"  

- Vortex/1.0  

- Apple-iPad1C1/812.1  

- Apple-iPhone3C1/811.2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5.1  

- SAMSUNGSPHD700/100.202  

- Android/0,3  

  También es posible que este valor esté vacío.  

### <a name="x-ms-proxy"></a>X-MS-proxy  
 Tipo de Claim: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 Esta AD FS notificaciones indica que la solicitud ha pasado a través del proxy de aplicación Web.  Esta solicitud se rellena mediante el proxy de aplicación Web, que rellena el encabezado al pasar la solicitud de autenticación al back-end Servicio de federación. A continuación, AD FS lo convierte en una demanda.  

 El valor de la demanda es el nombre DNS del proxy de aplicación web que pasó la solicitud.  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo de Claim: `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 Similar al tipo de notificaciones x-MS-proxy anterior, este tipo de demanda indica si la solicitud se ha pasado a través del proxy de aplicación Web. A diferencia de x-MS-proxy, insidecorporatenetwork es un valor booleano con true que indica una solicitud directamente al servicio de Federación desde dentro de la red corporativa.  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (activo frente a pasivo)  
 Tipo de Claim: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 Este tipo de notificaciones se puede usar para determinar las solicitudes que se originan en clientes "activos" (enriquecidos) frente a clientes "pasivos" (basados en explorador Web). Esto permite que se permitan las solicitudes externas desde aplicaciones basadas en explorador como Outlook Web Access, SharePoint Online o el portal de Office 365 mientras se bloquean las solicitudes procedentes de clientes enriquecidos como Microsoft Outlook.  

 El valor de la demanda es el nombre del servicio AD FS que recibió la solicitud.  

## <a name="see-also"></a>Consulta también  
 [Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
