---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Directivas de Control de acceso en AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc3de72aa993546151b103d6eeacd160f81a4270
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444384"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Directivas de Control de acceso en Windows Server 2012 R2 y Windows Server 2012 AD FS


Las directivas descritas en este artículo hacen que el uso de dos tipos de notificaciones  

1.  Las notificaciones de que AD FS crea basándose en la información del proxy de AD FS y la aplicación Web puede inspeccionar y comprobar, por ejemplo, la dirección IP del cliente que se conecta directamente a AD FS o el WAP.  

2.  Notificaciones de AD FS crea basándose en la información transmitida a AD FS por el cliente como encabezados HTTP  

>**Importantes**: Las directivas, tal como se describe a continuación se bloquea la unión a un dominio de Windows 10 e inicie sesión en escenarios que requieren acceso a los siguientes puntos de conexión adicionales

Puntos de conexión de AD FS necesarios para la unión de dominios de Windows 10 e inicie sesión en
- [federation service name]/adfs/services/trust/2005/windowstransport
- [nombre de servicio de federación] / adfs/services/trust/13/windowstransport
- [federation service name]/adfs/services/trust/2005/usernamemixed
- [federation service name]/adfs/services/trust/13/usernamemixed 
- [nombre de servicio de federación] / adfs/services/trust/2005/certificatemixed
- [nombre de servicio de federación] / adfs/services/trust/13/certificatemixed

Para resolver, actualice las directivas que deniegan en función de la notificación de punto de conexión para permitir excepción de los puntos de conexión anteriores.

Por ejemplo, la regla siguiente:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

se actualizaría para:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Notificaciones de esta categoría solo deben usarse para implementar directivas empresariales y no como directivas de seguridad para proteger el acceso a la red.  Es posible que los clientes no autorizados envíen encabezados con información falsa como una forma de obtener acceso.  

Siempre se deben usar las directivas descritas en este artículo con otro método de autenticación, como la autenticación de factor de nombre de usuario y una contraseña o múltiple.  

## <a name="client-access-policies-scenarios"></a>Escenarios de directivas de acceso de cliente  

|**Escenario**|**Descripción**| 
| --- | --- | 
|Escenario 1: Bloquear todo acceso externo a Office 365|Se permite el acceso a Office 365 desde todos los clientes en la red corporativa interna, pero se denegarán las solicitudes de clientes externos según la dirección IP del cliente externo.|  
|Escenario 2: Bloquear todo acceso externo a Office 365 excepto Exchange ActiveSync|Se permite el acceso de Office 365 desde todos los clientes en la red corporativa interna, así como de los dispositivos cliente externo, como los teléfonos inteligentes, que hacen uso de Exchange ActiveSync. Todos los otros clientes externos, como aquellas que usan Outlook, se bloquean.|  
|Escenario 3: Bloquear todo acceso externo a Office 365 excepto las aplicaciones basadas en explorador|Bloquea el acceso externo a Office 365, excepto para aplicaciones pasivas (basada en el explorador), como Outlook Web Access o SharePoint Online.|  
|Escenario 4: Bloquear todo acceso externo a Office 365, excepto los grupos de Active Directory designados|Este escenario se usa para probar y validar la implementación de directivas de acceso de cliente. Bloquea el acceso externo a Office 365 solo para los miembros de uno o más grupos de Active Directory. También puede utilizarse para proporcionar acceso externo solo a los miembros de un grupo.|  

## <a name="enabling-client-access-policy"></a>Habilitar directiva de acceso de cliente  
 Para habilitar la directiva de acceso de cliente de AD FS en Windows Server 2012 R2, debe actualizar la plataforma de identidad de Microsoft Office 365 confianza. Elija uno de los escenarios de ejemplo siguientes para configurar las reglas de notificación en el **plataforma de identidad de Microsoft Office 365** de confianza que mejor satisfaga las necesidades de su organización.  

###  <a name="scenario1"></a> Escenario 1: Bloquear todo acceso externo a Office 365  
 Este escenario de directiva de acceso de cliente permite el acceso de todos los clientes internos y bloques de todos los clientes externos según la dirección IP del cliente externo. Puede usar los procedimientos siguientes para agregar las reglas de autorización de emisión correctas a la confianza para la situación elegida de Office 365.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Para crear reglas para bloquear todo acceso externo a Office 365  

1.  Desde **administrador del servidor**, haga clic en **herramientas**, a continuación, haga clic en **administración de AD FS**.  

2.  En el árbol de consola, bajo **AD Fs\relaciones**, haga clic en **autenticado**, haga clic en el **plataforma de identidad de Microsoft Office 365** confiar y, a continuación, haga clic en **Editar reglas de notificación**.  

3.  En el **editar reglas de notificación** cuadro de diálogo, seleccione el **reglas de autorización de emisión** pestaña y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para la regla de notificación.  

4.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "Si no hay ninguna notificación IP fuera del rango deseado, denegar". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación (sustituya el valor por encima de la "x-ms-reenviados-client-ip" con una expresión válida de IP):  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista de reglas de autorización de emisión antes de que el valor predeterminado **permitir el acceso a todos los usuarios** regla (la regla de denegación tendrá prioridad aunque aparezca en la lista anteriormente).  Si no tiene el valor predeterminado Permitir regla de acceso, puede agregar uno al final de la lista mediante el lenguaje de reglas de notificación como sigue:  </br>

    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Para guardar las nuevas reglas, en el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar**. La lista resultante debe ser similar al siguiente.  

     ![Reglas de autorización de emisión](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario2"></a> Escenario 2: Bloquear todo acceso externo a Office 365 excepto Exchange ActiveSync  
 El ejemplo siguiente permite el acceso a todas las aplicaciones de Office 365, incluidos Exchange Online, de los clientes internos, incluido Outlook. Bloquea el acceso de los clientes que se encuentran fuera de la red corporativa, tal y como indica la dirección IP del cliente, excepto para los clientes de Exchange ActiveSync, como teléfonos inteligentes.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Para crear reglas para bloquear todo acceso externo a Office 365 excepto Exchange ActiveSync  

1.  Desde **administrador del servidor**, haga clic en **herramientas**, a continuación, haga clic en **administración de AD FS**.  

2.  En el árbol de consola, bajo **AD Fs\relaciones**, haga clic en **autenticado**, haga clic en el **plataforma de identidad de Microsoft Office 365** confiar y, a continuación, haga clic en **Editar reglas de notificación**.  

3.  En el **editar reglas de notificación** cuadro de diálogo, seleccione el **reglas de autorización de emisión** pestaña y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para la regla de notificación.  

4.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "Si no hay ninguna notificación IP fuera del rango deseado, emitir ipoutsiderange notificación". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación (sustituya el valor por encima de la "x-ms-reenviados-client-ip" con una expresión válida de IP):  

    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en el **reglas de autorización de emisión** lista.  

7.  A continuación, en el **editar reglas de notificación** cuadro de diálogo el **reglas de autorización de emisión** , haga clic **Agregar regla** volver a iniciar el Asistente para la regla de notificación.  

8.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

9. En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "si hay una dirección IP fuera del intervalo deseado y no hay una notificación de x-ms-client-application no EAS, denegar ". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en el **reglas de autorización de emisión** lista.  

11. A continuación, en el **editar reglas de notificación** cuadro de diálogo el **reglas de autorización de emisión** , haga clic **Agregar regla** volver a iniciar el Asistente para la regla de notificación.  

12. En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

13. En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "comprobar si existe la notificación de aplicación". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación:  

   ```  
   NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en el **reglas de autorización de emisión** lista.  

15. A continuación, en el **editar reglas de notificación** cuadro de diálogo el **reglas de autorización de emisión** , haga clic **Agregar regla** volver a iniciar el Asistente para la regla de notificación.  

16. En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

17. En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "denegar a los usuarios con ipoutsiderange true y la aplicación producirá un error". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación:  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece inmediatamente debajo de la regla anterior y antes de regla en el valor predeterminado de permitir el acceso a todos los usuarios en la lista de reglas de autorización de emisión (la regla de denegación tendrá prioridad aunque aparezca en la lista anteriormente).  </br>Si no tiene el valor predeterminado Permitir regla de acceso, puede agregar uno al final de la lista mediante el lenguaje de reglas de notificación como sigue:</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Para guardar las nuevas reglas, en el **editar reglas de notificación** diálogo cuadro, haga clic en Aceptar. La lista resultante debe ser similar al siguiente.  

    ![Reglas de autorización de emisión](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario3"></a> Escenario 3: Bloquear todo acceso externo a Office 365 excepto las aplicaciones basadas en explorador  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para crear reglas para bloquear todo acceso externo a Office 365 excepto las aplicaciones basadas en explorador  

1.  Desde **administrador del servidor**, haga clic en **herramientas**, a continuación, haga clic en **administración de AD FS**.  

2.  En el árbol de consola, bajo **AD Fs\relaciones**, haga clic en **autenticado**, haga clic en el **plataforma de identidad de Microsoft Office 365** confiar y, a continuación, haga clic en **Editar reglas de notificación**.  

3.  En el **editar reglas de notificación** cuadro de diálogo, seleccione el **reglas de autorización de emisión** pestaña y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para la regla de notificación.  

4.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "Si no hay ninguna notificación IP fuera del rango deseado, emitir ipoutsiderange notificación". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación (sustituya el valor por encima de la "x-ms-reenviados-client-ip" con una expresión válida de IP):  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en el **reglas de autorización de emisión** lista.  

7.  A continuación, en el **editar reglas de notificación** cuadro de diálogo el **reglas de autorización de emisión** , haga clic **Agregar regla** volver a iniciar el Asistente para la regla de notificación.  

8.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

9. En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "si hay una dirección IP fuera del intervalo deseado y el punto de conexión no es/adfs/ls, denegar". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en la lista de reglas de autorización de emisión antes de que el valor predeterminado **permitir el acceso a todos los usuarios** regla (la regla de denegación tendrá prioridad aunque aparezca en la lista anteriormente).  </br></br> Si no tiene el valor predeterminado Permitir regla de acceso, puede agregar uno al final de la lista mediante el lenguaje de reglas de notificación como sigue:  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Para guardar las nuevas reglas, en el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar**. La lista resultante debe ser similar al siguiente.  

    ![Emisión](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario4"></a> Escenario 4: Bloquear todo acceso externo a Office 365, excepto los grupos de Active Directory designados  
 El ejemplo siguiente habilita el acceso de clientes internos en función de la dirección IP. Bloquea el acceso de los clientes que se encuentran fuera de la red corporativa que tengan una dirección IP de cliente externo, excepto para aquellas personas en un Group.Use de Active Directory especificado las reglas de los pasos siguientes para agregar la autorización de emisión correcto a la  **Plataforma de identidad de Microsoft Office 365** confianza mediante el Asistente para la regla de notificación:  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Para crear reglas para bloquear todo acceso externo a Office 365, excepto para designado grupos de Active Directory  

1.  Desde **administrador del servidor**, haga clic en **herramientas**, a continuación, haga clic en **administración de AD FS**.  

2.  En el árbol de consola, bajo **AD Fs\relaciones**, haga clic en **autenticado**, haga clic en el **plataforma de identidad de Microsoft Office 365** confiar y, a continuación, haga clic en **Editar reglas de notificación**.  

3.  En el **editar reglas de notificación** cuadro de diálogo, seleccione el **reglas de autorización de emisión** pestaña y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para la regla de notificación.  

4.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

5.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "Si no hay ninguna notificación IP fuera del rango deseado, emitir notificaciones ipoutsiderange." En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación (sustituya el valor por encima de la "x-ms-reenviados-client-ip" con una expresión válida de IP):  


~~~
`c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en el **reglas de autorización de emisión** lista.  

7. A continuación, en el **editar reglas de notificación** cuadro de diálogo el **reglas de autorización de emisión** , haga clic **Agregar regla** volver a iniciar el Asistente para la regla de notificación.  

8. En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

9. En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "comprobación de SID de grupo". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación (reemplace "groupsid" con el SID real del grupo de AD que usa):  

    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece en el **reglas de autorización de emisión** lista.  

11. A continuación, en el **editar reglas de notificación** cuadro de diálogo el **reglas de autorización de emisión** , haga clic **Agregar regla** volver a iniciar el Asistente para la regla de notificación.  

12. En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación,** seleccione **enviar notificaciones mediante regla personalizada**y, a continuación, haga clic en **siguiente**.  

13. En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla, por ejemplo "denegar a los usuarios con ipoutsiderange true y groupsid producirá un error". En **regla personalizada**, escriba o pegue la siguiente sintaxis de lenguaje de reglas de notificación:  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Haga clic en **Finalizar**. Compruebe que la nueva regla aparece inmediatamente debajo de la regla anterior y antes de regla en el valor predeterminado de permitir el acceso a todos los usuarios en la lista de reglas de autorización de emisión (la regla de denegación tendrá prioridad aunque aparezca en la lista anteriormente).  </br></br>Si no tiene el valor predeterminado Permitir regla de acceso, puede agregar uno al final de la lista mediante el lenguaje de reglas de notificación como sigue:  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Para guardar las nuevas reglas, en el **editar reglas de notificación** diálogo cuadro, haga clic en Aceptar. La lista resultante debe ser similar al siguiente.  

     ![Emisión](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="buildingip"></a> Creación de la expresión de intervalo de direcciones IP  
 La notificación de x-ms-reenviados-ip del cliente se rellena con un encabezado HTTP que está establecido actualmente solo mediante Exchange Online, que llena el encabezado al pasar la solicitud de autenticación para AD FS. El valor de la notificación puede ser uno de los siguientes:  

> [!NOTE]
>  Exchange Online solo direcciones IPV4 e IPV6 no admite actualmente.  

-   Una única dirección IP: La dirección IP del cliente que se conecta directamente a Exchange Online  

> [!NOTE]
> - La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa de la puerta de enlace o proxy de salida de la organización.  
>   -   Los clientes que están conectados a la red corporativa mediante una VPN o por Microsoft DirectAccess (DA) pueden aparecer como clientes corporativos internos o clientes externos según la configuración de VPN o DA.  

-   Una o varias direcciones IP: Cuando Exchange Online no puede determinar la dirección IP del cliente que se conecta, establecerá el valor en función del valor del encabezado x-forwarded-for, un encabezado no estándar que puede incluirse en las solicitudes basadas en HTTP y es compatible con muchos clientes, los equilibradores de carga y servidores proxy en el mercado.  

> [!NOTE]
> 1. Varias direcciones IP, que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud, estarán separadas por una coma.  
>    2. Las direcciones IP relacionadas con la infraestructura de Exchange Online no funcionará en la lista.  

### <a name="regular-expressions"></a>Expresiones regulares  
 Una vez para que coincida con un intervalo de direcciones IP, es necesario construir una expresión regular para llevar a cabo la comparación. En la siguiente serie de pasos, proporcionamos ejemplos sobre cómo construir una expresión para que coincida con los siguientes intervalos de direcciones (tenga en cuenta que tendrá que cambiar estos ejemplos para que coincida con el intervalo IP público):  

- 192.168.1.1 – 192.168.1.25  

- 10.0.0.1 – 10.0.0.14  

  En primer lugar, el patrón básico que coincidirá con una única dirección IP es como sigue: \b###\\. ###\\. ###\\. ### \b  

  Al ampliar esto, podemos hacer coincidir dos direcciones IP diferentes con una expresión OR como sigue: \b###\\. ###\\. ###\\. ### \b&#124;\b###\\. ###\\. ###\\. ### \b  

  Por lo tanto, sería un ejemplo para que coincida con solo dos direcciones (por ejemplo, 192.168.1.1 o 10.0.0.1): \b192\\.168\\.1\\. 1\b&#124;\b10\\.0\\.0\\. 1\b  

  Esto le ofrece la técnica que puede escribir cualquier número de direcciones. Cuando un intervalo de direcciones deben permitir, por ejemplo, 192.168.1.1 – 192.168.1.25, la coincidencia debe realizarse carácter por carácter: \b192\\.168\\.1\\. () [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b  

  Tenga en cuenta lo siguiente:  

- La dirección IP se trata como cadena y no un número.  

- La regla se desglosa como sigue: \b192\\.168\\.1\\.  

- Esto coincide con cualquier valor que empieza por 192.168.1.  

- El siguiente coincide con los intervalos necesarios para la parte de la dirección después del separador decimal final:  

  -   ([1-9] coincide con las direcciones que terminen en 1-9  

  -   &#124;1 [0-9] coincide con las direcciones que terminan en 10-19  

  -   &#124;Direcciones de coincidencias de 2[0-5]) terminen en 20 a 25  

- Tenga en cuenta que los paréntesis se deben colocar correctamente, no se inician otras partes de direcciones IP coincidentes.  

- Con el bloque 192 coincidente, podemos escribir una expresión similar para el bloque de 10: \b10\\.0\\.0\\. () [1-9] &#124;1 [0-4]) \b  

- Y colocarlos juntos, la siguiente expresión debe coincidir todas las direcciones de "192.168.1.1~25" y "10.0.0.1~14": \b192\\.168\\.1\\. () [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\.0\\.0\\. () [1-9] &#124;1 [0-4]) \b  

### <a name="testing-the-expression"></a>Probar la expresión  
 Las expresiones de Regex pueden ser bastante complicadas, por lo que se recomienda usar una herramienta de comprobación de regex. Si lo hace una búsqueda de "generador de expresiones de regex en línea" internet, encontrará varias utilidades buena en línea que le permitirá probar las expresiones con datos de ejemplo.  

 Al probar la expresión, es importante que comprenda lo que puede esperar que deben coincidir. El sistema de Exchange online puede enviar muchas direcciones IP separadas por comas. Las expresiones proporcionadas anteriormente funcionará para esto. Sin embargo, es importante pensar en esto al probar las expresiones de regex. Por ejemplo, se podría usar el siguiente ejemplo de entrada para comprobar los ejemplos anteriores:  

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  

## <a name="claim-types"></a>Tipos de notificación  
 AD FS en Windows Server 2012 R2 proporciona la información de contexto de solicitud utilizando los siguientes tipos de notificación:  

### <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP  
 Tipo de notificación: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 Esta notificación de AD FS representa un "mejor intento" en determinar la dirección IP del usuario (por ejemplo, el cliente de Outlook) que realiza la solicitud. Esta notificación puede contener varias direcciones IP, incluida la dirección de cada servidor proxy que reenvía la solicitud.  Esta notificación se rellena con HTTP. El valor de la notificación puede ser uno de los siguientes:  

-   Una única dirección IP: dirección IP del cliente que está conectado directamente a Exchange Online  

> [!NOTE]
>  La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa de la puerta de enlace o proxy de salida de la organización.  

-   Una o varias direcciones IP  

    -   Si no se puede determinar la dirección IP del cliente de conexión de Exchange Online, establecerá el valor según el valor del encabezado x-forwarded-for, las solicitudes de un encabezado no estándar que puede incluirse en basado en HTTP y es compatible con muchos clientes, equilibradores de carga, y servidores proxy en el mercado.  

    -   Varias direcciones IP que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud estarán separadas por una coma.  

> [!NOTE]
>  Las direcciones IP relacionadas con la infraestructura de Exchange Online no estará presentes en la lista.  

> [!WARNING]
>  Exchange Online actualmente admite solo las direcciones IPV4; no admite direcciones IPV6.  

### <a name="x-ms-client-application"></a>X-MS-Client-Application  
 Tipo de notificación: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 Esta notificación de AD FS representa el protocolo utilizado por el cliente final, que imprecisa corresponde a la aplicación que se va a usar.  Esta notificación se rellena a partir de un encabezado HTTP que está actualmente establecido sólo por Exchange Online, que llena el encabezado al pasar la solicitud de autenticación para AD FS. Dependiendo de la aplicación, el valor de esta notificación será uno de los siguientes:  

-   En el caso de los dispositivos que usan Exchange Active Sync, el valor es Microsoft.Exchange.ActiveSync.  

-   Uso del cliente Microsoft Outlook puede producir en cualquiera de los siguientes valores:  

    -   Microsoft.Exchange.Autodiscover  

    -   Microsoft.Exchange.OfflineAddressBook  

    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  

    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  

-   Otros valores posibles para este encabezado incluyen lo siguiente:  

    -   Microsoft.Exchange.Powershell  

    -   Microsoft.Exchange.SMTP  

    -   Microsoft.Exchange.Pop  

    -   Microsoft.Exchange.Imap  

### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 Tipo de notificación: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 Esta notificación de AD FS proporciona una cadena que represente el tipo de dispositivo que usa el cliente para tener acceso al servicio. Esto se puede usar cuando los clientes deseen evitar el acceso a determinados dispositivos (por ejemplo, determinados tipos de teléfonos inteligentes).  Los valores de esta notificación de ejemplo incluyen (pero no están limitados a) los siguientes valores.  

 El a continuación se muestran algunos ejemplos de lo que podría contener el valor de x-ms-user-agent para un cliente cuyo x-ms-client-application es "Microsoft.Exchange.ActiveSync"  

- Vortex/1.0  

- Apple-iPad1C1/812.1  

- Apple-iPhone3C1/811.2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5.1  

- SAMSUNGSPHD700/100.202  

- Android/0.3  

  También es posible que este valor está vacío.  

### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Tipo de notificación: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 Esta notificación de AD FS indica que la solicitud se pasa a través del proxy de aplicación Web.  Esta notificación se rellena con el proxy de aplicación Web, que rellena el encabezado al pasar la solicitud de autenticación para el back-end del servicio de federación. AD FS, a continuación, lo convierte en una notificación.  

 El valor de la notificación es el nombre DNS del proxy de aplicación Web que pasa la solicitud.  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo de notificación: `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 Tipo de notificación de forma similar a la anterior x-ms-proxy, este tipo de notificación indica si la solicitud se pasa a través del proxy de aplicación web. A diferencia de x-ms-proxy insidecorporatenetwork es un valor booleano True que indica una solicitud directamente al servicio de federación desde dentro de la red corporativa.  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-absoluto-Path (activa o pasiva)  
 Tipo de notificación: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 Este tipo de notificación puede utilizarse para determinar las solicitudes procedentes de clientes (avanzados) "activos" frente a los clientes "pasivos" (explorador basadas en web). Esto permite que las solicitudes externas desde aplicaciones basadas en explorador como Outlook Web Access, SharePoint Online o el portal de Office 365 se permite mientras se bloquean las solicitudes procedentes de clientes enriquecidos, como Microsoft Outlook.  

 El valor de la notificación es el nombre del servicio AD FS que recibió la solicitud.  

## <a name="see-also"></a>Vea también  
 [Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
