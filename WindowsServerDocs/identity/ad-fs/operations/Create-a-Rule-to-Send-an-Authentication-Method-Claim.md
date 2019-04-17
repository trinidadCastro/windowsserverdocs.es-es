---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: "Crear una regla para enviar una notificación de método de autenticación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Crear una regla para enviar una notificación de método de autenticación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Puedes usar cualquiera el **enviar pertenencia a grupos como notificaciones** plantilla de regla o **transformar una notificación entrante** plantilla regla para enviar una notificación de método de autenticación. El usuario de confianza puede usar una notificación de método de autenticación para determinar el mecanismo de inicio de sesión que el usuario se usa para autenticar y obtener las notificaciones de los servicios de federación de Active Directory \(AD FS\). También puedes usar la característica de mecanismo de autenticación de los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 R2 como entrada para generar notificaciones de método de autenticación para situaciones en que quiere que el usuario de confianza determinar el nivel de acceso que se basa en los inicios de sesión de tarjeta inteligente. Por ejemplo, un desarrollador puede asignar diferentes niveles de acceso a los usuarios federados de la aplicación de terceros de confianza. Los niveles de acceso se basan en si los usuarios iniciar sesión con sus credenciales de nombre y la contraseña de usuario, en lugar de sus tarjetas inteligentes.  
  
Según los requisitos de la organización, usa uno de los procedimientos siguientes:  
  
-   Crear esta regla mediante la **enviar pertenencia a grupos como reclamaciones** plantilla regla \-puedes usar esta plantilla de regla cuando quieras que el grupo que se especifique en esta plantilla para determinar qué método de autenticación de finalmente reclamar emitir.  
  
-   Crear esta regla mediante la **transformar una notificación entrante** plantilla regla \-puedes usar esta plantilla de regla cuando quieras cambiar el método de autenticación existente a un nuevo método de autenticación que funciona con un producto que no reconoce estándar reclamaciones de método de autenticación de AD FS.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear mediante la pertenencia al grupo de enviar como plantilla de regla de notificaciones en una confianza del fabricante de confiar en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar pertenencia a grupos como notificación** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  Haz clic en **examinar**, selecciona el grupo cuyos miembros deben recibir esta notificación del método de autenticación y, a continuación, haz clic en **Aceptar**.  
  
8.  En **tipo de notificación saliente**, selecciona **método de autenticación** en la lista.  
  
9. En **saliente el valor de notificación**, escriba uno de los valores \(URI\) de identificador de uniforme de recursos de manera predeterminada en la siguiente tabla, según el método de autenticación preferido, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre y la contraseña de usuario|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Seguridad de la capa \(TLS\) la autenticación mutua que usa certificados X.509 de transporte|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticación basada en X.509\ que no use TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear la regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear mediante la pertenencia al grupo de enviar como plantilla de regla de notificaciones en un proveedor de notificaciones de confianza en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar pertenencia a grupos como notificación** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  Haz clic en **examinar**, selecciona el grupo cuyos miembros deben recibir esta notificación del método de autenticación y, a continuación, haz clic en **Aceptar**.  
  
8.  En **tipo de notificación saliente**, selecciona **método de autenticación** en la lista.  
  
9. En **saliente el valor de notificación**, escriba uno de los valores \(URI\) de identificador de uniforme de recursos de manera predeterminada en la siguiente tabla, según el método de autenticación preferido, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre y la contraseña de usuario|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Seguridad de la capa \(TLS\) la autenticación mutua que usa certificados X.509 de transporte|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticación basada en X.509\ que no use TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear la regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear esta regla mediante la transformación un entrante reclama la plantilla de la regla en una confianza del fabricante de confiar en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **transformar una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona **método de autenticación** en la lista.  
  
8.  En **tipo de notificación saliente**, selecciona **método de autenticación** en la lista.  
  
9. Selecciona **reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**, y, a continuación, haz lo siguiente:  
  
    1.  En **el valor de notificación entrante**, escriba uno de los siguientes valores URI que se basan en el método de autenticación real URI que se usó originalmente, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
    2.  En **saliente el valor de notificación**, escriba uno de los valores URI de forma predeterminada en la siguiente tabla, que depende de tu elección de método de autenticación preferido nuevo, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre y la contraseña de usuario|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticación mutua TLS que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticación basada en X.509\ que no use TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear la regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Otros valores de identificador URI pueden usarse además de los valores en la tabla. Los valores URI que se muestran iones en la tabla anterior se reflejan los URI que el usuario de confianza acepta de forma predeterminada.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear esta regla mediante la transformación un entrante reclama la plantilla de la regla en una confianza de proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **transformar una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona **método de autenticación** en la lista.  
  
8.  En **tipo de notificación saliente**, selecciona **método de autenticación** en la lista.  
  
9. Selecciona **reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**, y, a continuación, haz lo siguiente:  
  
    1.  En **el valor de notificación entrante**, escriba uno de los siguientes valores URI que se basan en el método de autenticación real URI que se usó originalmente, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
    2.  En **saliente el valor de notificación**, escriba uno de los valores URI de forma predeterminada en la siguiente tabla, que depende de tu elección de método de autenticación preferido nuevo, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre y la contraseña de usuario|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticación mutua TLS que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticación basada en X.509\ que no use TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear la regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Para crear esta regla mediante la pertenencia al grupo de enviar como plantilla de regla de notificaciones en Windows Server 2012 R2  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS\\Trust relaciones**, haz clic en **reclamaciones proveedor confía** o **confiar confía en las partes**y, a continuación, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  En la **editar reglas de notificación** cuadro de diálogo, selecciona una de las siguientes pestañas, según la confianza de que vas a editar y establece qué regla quieras crear esta regla en y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar pertenencia a grupos como notificación** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  Haz clic en **examinar**, selecciona el grupo cuyos miembros deben recibir esta notificación del método de autenticación y, a continuación, haz clic en **Aceptar**.  
  
8.  En **tipo de notificación saliente**, selecciona **método de autenticación** en la lista.  
  
9. En **saliente el valor de notificación**, escriba uno de los valores \(URI\) de identificador de uniforme de recursos de manera predeterminada en la siguiente tabla, según el método de autenticación preferido, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre y la contraseña de usuario|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Seguridad de la capa \(TLS\) la autenticación mutua que usa certificados X.509 de transporte|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticación basada en X.509\ que no use TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear la regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Otros valores de identificador URI pueden usarse además de los valores en la tabla. Los valores URI que se muestran en la tabla anterior reflejan a los URI que el usuario de confianza acepta de forma predeterminada.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para crear esta regla usando la transformación de una plantilla de regla de notificación entrante en Windows Server 2012 R2  
   
  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **AD FS administración **.  
  
2.  En el árbol de consola, en **AD FS\\Trust relaciones**, haz clic en **reclamaciones proveedor confía** o **confiar confía en las partes**y, a continuación, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  En la **editar reglas de notificación** cuadro de diálogo, selecciona una las fichas siguientes, que depende de la confianza de que está editando y en la regla que establece quieran crear esta regla y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **transformar una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona **método de autenticación** en la lista.  
  
8.  En **tipo de notificación saliente**, selecciona **método de autenticación** en la lista.  
  
9. Selecciona **reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**, y, a continuación, haz lo siguiente:  
  
    1.  En **el valor de notificación entrante**, escriba uno de los siguientes valores URI que se basan en el método de autenticación real URI que se usó originalmente, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
    2.  En **saliente el valor de notificación**, escriba uno de los valores URI de forma predeterminada en la siguiente tabla, que depende de tu elección de método de autenticación preferido nuevo, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre y la contraseña de usuario|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticación mutua TLS que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticación basada en X.509\ que no use TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear la regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Otros valores de identificador URI pueden usarse además de los valores en la tabla. Los valores URI que se muestran iones en la tabla anterior se reflejan los URI que el usuario de confianza acepta de forma predeterminada.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
