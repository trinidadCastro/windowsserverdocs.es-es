---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Crear una regla para enviar una notificación de método de autenticación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be3a16bac9c146637117aa7b9720cb4aa76177e2
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189390"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Crear una regla para enviar una notificación de método de autenticación


Puede usar el **enviar pertenencia a grupos como notificaciones** plantilla de regla o el **transformar una notificación entrante** plantilla de regla para enviar una notificación del método de autenticación. El usuario de confianza puede usar una notificación del método de autenticación para determinar el mecanismo de inicio de sesión que el usuario utiliza para autenticar y obtener notificaciones de Active Directory Federation Services \(AD FS\). También puede usar la característica de seguridad del mecanismo de autenticación de Active Directory Federation Services \(AD FS\) en Windows Server 2012 R2 como entrada para generar notificaciones de método de autenticación para las situaciones en que el usuario de confianza ¿desea determinar el nivel de acceso que se basa en inicios de sesión de tarjeta inteligente. Por ejemplo, un desarrollador puede asignar distintos niveles de acceso a los usuarios federados de la aplicación del usuario autenticado. Los niveles de acceso se basan en si los usuarios iniciar sesión con sus credenciales de nombre y la contraseña de usuario, en lugar de sus tarjetas inteligentes.  
  
Según los requisitos de su organización, use uno de los siguientes procedimientos:  
  
-   Crear esta regla mediante la **enviar pertenencia a grupos como notificaciones** plantilla de regla \- puede usar esta plantilla de regla cuando desee que el grupo que especifique en última instancia de esta plantilla para determinar qué método de autenticación notificación emitir.  
  
-   Crear esta regla mediante la **transformar una notificación entrante** plantilla de regla \- puede usar esta plantilla de regla cuando desea cambiar el método de autenticación existente a un nuevo método de autenticación que funciona con un producto no reconoce estándares notificaciones de método de autenticación de AD FS.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear mediante la pertenencia a enviar como plantilla de reglas de notificaciones en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar pertenencia a grupo como notificación** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  Haga clic en **examinar**, seleccione el grupo cuyos miembros deben recibir esta notificación de método de autenticación y, a continuación, haga clic en **Aceptar**.  
  
8.  En **tipo de notificación saliente**, seleccione **método de autenticación** en la lista.  
  
9. En **valor de notificación saliente**, escriba uno de identificador uniforme de recursos predeterminado \(URI\) valores en la tabla siguiente, dependiendo del método de autenticación preferido, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre de usuario y contraseña|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows.|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Seguridad de la capa de transporte \(TLS\) autenticación mutua que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en función de autenticación que no usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear mediante la pertenencia a enviar como plantilla de reglas de notificaciones en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar pertenencia a grupo como notificación** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  Haga clic en **examinar**, seleccione el grupo cuyos miembros deben recibir esta notificación de método de autenticación y, a continuación, haga clic en **Aceptar**.  
  
8.  En **tipo de notificación saliente**, seleccione **método de autenticación** en la lista.  
  
9. En **valor de notificación saliente**, escriba uno de identificador uniforme de recursos predeterminado \(URI\) valores en la tabla siguiente, dependiendo del método de autenticación preferido, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre de usuario y contraseña|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows.|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Seguridad de la capa de transporte \(TLS\) autenticación mutua que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en función de autenticación que no usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear esta regla mediante la transformación de una notificación entrante plantilla de regla en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **transformar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, seleccione **método de autenticación** en la lista.  
  
8.  En **tipo de notificación saliente**, seleccione **método de autenticación** en la lista.  
  
9. Seleccione **reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**, y, a continuación, haga lo siguiente:  
  
    1.  En **valor de notificación entrante**, escriba uno de los siguientes valores URI que se basan en el método de autenticación real URI que se usó originalmente, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  
  
    2.  En **valor de notificación saliente**, escriba uno de los valores URI de forma predeterminada en la tabla siguiente, que depende de su elección de método de autenticación preferido nueva, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**  para guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre de usuario y contraseña|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows.|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticación mutua de TLS que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en función de autenticación que no usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Otros valores URI se pueden usar además de los valores de la tabla. Los valores URI que se muestran ion de la tabla anterior refleja los URI que acepta el usuario de confianza de forma predeterminada.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear esta regla mediante la transformación de una notificación entrante plantilla de regla en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **transformar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, seleccione **método de autenticación** en la lista.  
  
8.  En **tipo de notificación saliente**, seleccione **método de autenticación** en la lista.  
  
9. Seleccione **reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**, y, a continuación, haga lo siguiente:  
  
    1.  En **valor de notificación entrante**, escriba uno de los siguientes valores URI que se basan en el método de autenticación real URI que se usó originalmente, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  
  
    2.  En **valor de notificación saliente**, escriba uno de los valores URI de forma predeterminada en la tabla siguiente, que depende de su elección de método de autenticación preferido nueva, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**  para guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre de usuario y contraseña|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows.|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticación mutua de TLS que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en función de autenticación que no usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Para crear esta regla mediante la pertenencia a enviar como plantilla de reglas de notificaciones en Windows Server 2012 R2  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o **autenticado**y, a continuación, haga clic en un determinado confiar en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  En el **editar reglas de notificación** cuadro de diálogo, seleccione una de las fichas siguientes, dependiendo de la relación de confianza que va a editar y establezca la regla a la desea crear esta regla en y, a continuación, haga clic en **Agregar regla** para iniciar la regla Asistente para la que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar pertenencia a grupo como notificación** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  Haga clic en **examinar**, seleccione el grupo cuyos miembros deben recibir esta notificación de método de autenticación y, a continuación, haga clic en **Aceptar**.  
  
8.  En **tipo de notificación saliente**, seleccione **método de autenticación** en la lista.  
  
9. En **valor de notificación saliente**, escriba uno de identificador uniforme de recursos predeterminado \(URI\) valores en la tabla siguiente, dependiendo del método de autenticación preferido, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre de usuario y contraseña|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows.|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Seguridad de la capa de transporte \(TLS\) autenticación mutua que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en función de autenticación que no usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Otros valores URI se pueden usar además de los valores de la tabla. Los valores URI que se muestran en la tabla anterior reflejan a los URI que acepta el usuario de confianza de forma predeterminada.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para crear esta regla mediante la transformación de una plantilla de regla de notificación entrante en Windows Server 2012 R2  
   
  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o **autenticado**y, a continuación, haga clic en un determinado confiar en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  En el **editar reglas de notificación** cuadro de diálogo, seleccione una las fichas siguientes, que depende de la relación de confianza que está editando y en qué regla establece desean crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar la regla Asistente para la que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **transformar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, seleccione **método de autenticación** en la lista.  
  
8.  En **tipo de notificación saliente**, seleccione **método de autenticación** en la lista.  
  
9. Seleccione **reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**, y, a continuación, haga lo siguiente:  
  
    1.  En **valor de notificación entrante**, escriba uno de los siguientes valores URI que se basan en el método de autenticación real URI que se usó originalmente, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  
  
    2.  En **valor de notificación saliente**, escriba uno de los valores URI de forma predeterminada en la tabla siguiente, que depende de su elección de método de autenticación preferido nueva, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**  para guardar la regla.  
  
|Método de autenticación real|URI correspondiente|  
|--------------------------------|---------------------|  
|Autenticación de nombre de usuario y contraseña|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticación de Windows.|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticación mutua de TLS que usa certificados X.509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-en función de autenticación que no usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Otros valores URI se pueden usar además de los valores de la tabla. Los valores URI que se muestran ion de la tabla anterior refleja los URI que acepta el usuario de confianza de forma predeterminada.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
