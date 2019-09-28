---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Crear una regla para enviar una notificación de método de autenticación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4efcae02b96904c9f869a5ed9e14eba161892b74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358161"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Crear una regla para enviar una notificación de método de autenticación


Puede usar la plantilla de regla **Enviar pertenencia a grupos como notificaciones** o la plantilla de regla **transformar una notificación entrante** para enviar una notificación de método de autenticación. El usuario de confianza puede usar una notificación de método de autenticación para determinar el mecanismo de inicio de sesión que el usuario usa para autenticar y obtener notificaciones de Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1. También puede usar la característica de comprobación del mecanismo de autenticación de Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 en Windows Server 2012 R2 como entrada para generar notificaciones de método de autenticación para las situaciones en las que el usuario de confianza desea determinar el nivel de acceso que se basa en inicios de sesión de tarjeta inteligente. Por ejemplo, un desarrollador puede asignar distintos niveles de acceso a usuarios federados de la aplicación de usuario de confianza. Los niveles de acceso se basan en si los usuarios inician sesión con sus credenciales de nombre de usuario y contraseña, en lugar de en sus tarjetas inteligentes.  

En función de los requisitos de su organización, use uno de los procedimientos siguientes:  

-   Cree esta regla mediante la plantilla de regla **Enviar pertenencia a grupos como notificaciones** \- puede usar esta plantilla de regla si desea que el grupo que especifique en esta plantilla determine en última instancia qué notificación del método de autenticación se va a emitir.  

-   Cree esta regla mediante la plantilla de regla **transformar una notificaciones entrantes** \- puede usar esta plantilla de regla si desea cambiar el método de autenticación existente a un nuevo método de autenticación que funcione con un producto que no reconozca notificaciones estándar AD FS método de autenticación.  



## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear mediante la plantilla de regla enviar pertenencia a grupos como notificaciones en una relación de confianza para usuario autenticado en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  Haga\-clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.  

7.  Haga clic en **examinar**, seleccione el grupo cuyos miembros deben recibir este método de autenticación y, a continuación, haga clic en **Aceptar**.  

8.  En **tipo de notificaciones salientes**, seleccione **método de autenticación** en la lista.  

9. En **valor de notificaciones salientes**, escriba uno de los valores predeterminados de identificador uniforme de recursos \(URI @ no__t-2 en la tabla siguiente, según el método de autenticación preferido, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

|                            Método de autenticación real                             |                                URI correspondiente                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Autenticación de nombre de usuario y contraseña                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Autenticación de Windows.                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Seguridad de la capa de transporte \(TLS @ no__t-1 autenticación mutua que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  Autenticación X. 509 @ no__t-0based que no usa TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)

## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear mediante la plantilla de regla enviar pertenencia a grupos como notificaciones en una confianza de proveedor de notificaciones en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  Haga\-clic con el botón secundario en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.  

7.  Haga clic en **examinar**, seleccione el grupo cuyos miembros deben recibir este método de autenticación y, a continuación, haga clic en **Aceptar**.  

8.  En **tipo de notificaciones salientes**, seleccione **método de autenticación** en la lista.  

9. En **valor de notificaciones salientes**, escriba uno de los valores predeterminados de identificador uniforme de recursos \(URI @ no__t-2 en la tabla siguiente, según el método de autenticación preferido, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

|                            Método de autenticación real                             |                                URI correspondiente                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Autenticación de nombre de usuario y contraseña                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Autenticación de Windows.                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Seguridad de la capa de transporte \(TLS @ no__t-1 autenticación mutua que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  Autenticación X. 509 @ no__t-0based que no usa TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear esta regla mediante la plantilla de regla transformar una notificaciones entrantes en una relación de confianza para usuario autenticado en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  Haga\-clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **transformar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.  

7.  En **tipo de notificaciones entrantes**, seleccione **método de autenticación** en la lista.  

8.  En **tipo de notificaciones salientes**, seleccione **método de autenticación** en la lista.  

9. Seleccione **reemplazar un valor de notificaciones entrantes por otro valor de notificaciones salientes**y, a continuación, haga lo siguiente:  

    1.  En **valor de notificaciones entrantes**, escriba uno de los siguientes valores de URI basados en el URI del método de autenticación real que se usó originalmente, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

    2.  En **valor de notificaciones salientes**, escriba uno de los valores de URI predeterminados en la tabla siguiente, que depende de la opción nuevo método de autenticación preferido, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

|              Método de autenticación real              |                                URI correspondiente                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Autenticación de nombre de usuario y contraseña          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Autenticación de Windows.                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Autenticación mutua de TLS que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   Autenticación X. 509 @ no__t-0based que no usa TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)

> [!NOTE]  
> Se pueden usar otros valores de URI además de los valores de la tabla. Los valores de URI que se muestran en la tabla anterior reflejan los URI que el usuario de confianza acepta de forma predeterminada.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear esta regla mediante la plantilla de regla transformar una notificación entrante en una confianza de proveedor de notificaciones en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  Haga\-clic con el botón secundario en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **transformar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.  

7.  En **tipo de notificaciones entrantes**, seleccione **método de autenticación** en la lista.  

8.  En **tipo de notificaciones salientes**, seleccione **método de autenticación** en la lista.  

9. Seleccione **reemplazar un valor de notificaciones entrantes por otro valor de notificaciones salientes**y, a continuación, haga lo siguiente:  

    1.  En **valor de notificaciones entrantes**, escriba uno de los siguientes valores de URI basados en el URI del método de autenticación real que se usó originalmente, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

    2.  En **valor de notificaciones salientes**, escriba uno de los valores de URI predeterminados en la tabla siguiente, que depende de la opción nuevo método de autenticación preferido, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

|              Método de autenticación real              |                                URI correspondiente                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Autenticación de nombre de usuario y contraseña          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Autenticación de Windows.                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Autenticación mutua de TLS que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   Autenticación X. 509 @ no__t-0based que no usa TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Para crear esta regla mediante la plantilla de regla enviar pertenencia a grupos como notificaciones en Windows Server 2012 R2  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En el árbol de consola, **en\\AD FS relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas**de usuario de confianza y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.  

3.  Haga\-clic con el botón secundario en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  

4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, en función de la confianza que esté editando y el conjunto de reglas en el que desee crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas. :  

    -   **Reglas de transformación de aceptación**  

    -   **Reglas de transformación de emisión**  

    -   **Reglas de autorización de emisión**  

    -   **Reglas de autorización de delegación**  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como una demanda** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.  

7.  Haga clic en **examinar**, seleccione el grupo cuyos miembros deben recibir este método de autenticación y, a continuación, haga clic en **Aceptar**.  

8.  En **tipo de notificaciones salientes**, seleccione **método de autenticación** en la lista.  

9. En **valor de notificaciones salientes**, escriba uno de los valores predeterminados de identificador uniforme de recursos \(URI @ no__t-2 en la tabla siguiente, según el método de autenticación preferido, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

|                            Método de autenticación real                             |                                URI correspondiente                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Autenticación de nombre de usuario y contraseña                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Autenticación de Windows.                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Seguridad de la capa de transporte \(TLS @ no__t-1 autenticación mutua que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  Autenticación X. 509 @ no__t-0based que no usa TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)

> [!NOTE]  
> Se pueden usar otros valores de URI además de los valores de la tabla. Los valores de URI que se muestran en la tabla anterior reflejan los URI que el usuario de confianza acepta de forma predeterminada.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para crear esta regla mediante la plantilla de reglas de transformación de notificaciones entrantes en Windows Server 2012 R2  



1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **Administración de AD FS**.  

2.  En el árbol de consola, **en\\AD FS relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas**de usuario de confianza y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.  

3.  Haga\-clic con el botón secundario en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 

4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, que depende de la confianza que está editando y en qué conjunto de reglas desea crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas. :  

    -   **Reglas de transformación de aceptación**  

    -   **Reglas de transformación de emisión**  

    -   **Reglas de autorización de emisión**  

    -   **Reglas de autorización de delegación**  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **transformar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.  

7.  En **tipo de notificaciones entrantes**, seleccione **método de autenticación** en la lista.  

8.  En **tipo de notificaciones salientes**, seleccione **método de autenticación** en la lista.  

9. Seleccione **reemplazar un valor de notificaciones entrantes por otro valor de notificaciones salientes**y, a continuación, haga lo siguiente:  

    1.  En **valor de notificaciones entrantes**, escriba uno de los siguientes valores de URI basados en el URI del método de autenticación real que se usó originalmente, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

    2.  En **valor de notificaciones salientes**, escriba uno de los valores de URI predeterminados en la tabla siguiente, que depende de la opción nuevo método de autenticación preferido, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

|              Método de autenticación real              |                                URI correspondiente                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Autenticación de nombre de usuario y contraseña          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Autenticación de Windows.                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Autenticación mutua de TLS que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   Autenticación X. 509 @ no__t-0based que no usa TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![crear regla](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)

> [!NOTE]  
> Se pueden usar otros valores de URI además de los valores de la tabla. Los valores de URI que se muestran en la tabla anterior reflejan los URI que el usuario de confianza acepta de forma predeterminada.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  

[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](https://technet.microsoft.com/library/ee913564.aspx)  

[Cuándo usar una regla de notificaciones de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  

[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
