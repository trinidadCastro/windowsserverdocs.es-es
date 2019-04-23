---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Crear una regla para transformar una notificación entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881046"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Crear una regla para enviar una notificación Compatible de AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2


En situaciones en que se usa Active Directory Federation Services \(AD FS\) para emitir notificaciones que se recibirán por servidores de federación con AD FS 1.0 \(Windows Server 2003 R2\) o AD FS 1.1 \(Windows Server 2008 o Windows Server 2008 R2\), debe hacer lo siguiente:  
  
-   Cree una regla que va a enviar un tipo de notificación de Id. de nombre con un formato de UPN, correo electrónico o nombre común.  
  
-   Todas las otras notificaciones que se envían deben tener uno de los siguientes tipos de notificación:  
  
    -   AD FS 1. *x* dirección de correo electrónico  
  
    -   AD FS 1. *x* UPN  
  
    -   Nombre común  
  
    -   Agrupar  
  
    -   Cualquier otro tipo de notificación que comienza con https://schemas.xmlsoap.org/claims/, como https://schemas.xmlsoap.org/claims/EmployeeID  
  
Según las necesidades de su organización, utilice uno de los procedimientos siguientes para crear AD FS 1. *x* notificación NameID compatible con:  
  
-   Crear esta regla de notificación de problema de AD FS 1.x Id. de nombre utilizando el **pasar o filtrar una plantilla de regla de notificación entrante**  
  
-   Crear esta regla de notificación de problema de AD FS 1.x Id. de nombre utilizando el **transformar una plantilla de regla de notificación entrante**. Puede usar esta plantilla de regla en situaciones en que desea cambiar el tipo de notificación existente a un nuevo tipo de notificación que funcione con AD FS 1.  *x* notificaciones.  
  
> [!NOTE]  
> Para que esta regla funcione según lo esperado, asegúrese de que la relación de confianza o relación de confianza de proveedor de notificaciones donde va a crear esta regla se ha configurado para usar el **perfil de AD FS 1.0 y 1.1**. 




## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para emitir AD FS 1. *x* Id. de nombre de notificación con el paso a través o filtrar una plantilla de regla de notificación entrante en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **pasar o filtrar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente** .  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, seleccione **Id. de nombre** en la lista.  
  
8.  En **formato de Id. de nombre entrantes**, seleccione uno de AD FS 1. *x*\-compatible con formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\-correo**  
  
    -   **Nombre común**  
  
9. Seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Pasar a través solo un determinado valor de notificación**  
  
    -   **Pasar a través de los valores de notificación que coincidan con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar a través de los valores de notificación que empiezan con un valor específico**  
![Crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para emitir AD FS 1. *x* Id. de nombre de notificación con el paso a través o filtrar una plantilla de regla de notificación entrante en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **pasar o filtrar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente** .  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, seleccione **Id. de nombre** en la lista.  
  
8.  En **formato de Id. de nombre entrantes**, seleccione uno de AD FS 1. *x*\-compatible con formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\-correo**  
  
    -   **Nombre común**  
  
9. Seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Pasar a través solo un determinado valor de notificación**  
  
    -   **Pasar a través de los valores de notificación que coincidan con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar a través de los valores de notificación que empiezan con un valor específico**  
![Crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

  

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en una confianza de Windows Server 2016 

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
  
7.  En **tipo de notificación entrante**, seleccione el tipo de notificación entrante que desea transformar en la lista.  
  
8.  En **tipo de notificación saliente**, seleccione **Id. de nombre** en la lista.  
  
9. En **formato de Id. de nombre saliente**, seleccione uno de AD FS 1. *x*\-compatible con formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\-correo**  
  
    -   **Nombre común**  
  
10. Seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**  
  
    -   **Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico**  
![Crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en una confianza del proveedor de notificaciones en Windows Server 2016 
  
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
  
7.  En **tipo de notificación entrante**, seleccione el tipo de notificación entrante que desea transformar en la lista.  
  
8.  En **tipo de notificación saliente**, seleccione **Id. de nombre** en la lista.  
  
9. En **formato de Id. de nombre saliente**, seleccione uno de AD FS 1. *x*\-compatible con formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\-correo**  
  
    -   **Nombre común**  
  
10. Seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**  
  
    -   **Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico**  
![Crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Para crear una regla para emitir AD FS 1. *x* Id. de nombre de notificación con el paso a través o filtrar una plantilla de regla de notificación entrante en Windows Server 2012 R2
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o **autenticado**y, a continuación, haga clic en un determinado confiar en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  En el **editar reglas de notificación** cuadro de diálogo, seleccione una de las fichas siguientes, dependiendo de la confianza que va a editar y establezca la regla a la desea crear esta regla en y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **pasar o filtrar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente** .  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)  
  
6.  En el **configurar regla** página, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, seleccione **Id. de nombre** en la lista.  
  
8.  En **formato de Id. de nombre entrantes**, seleccione uno de AD FS 1. *x*\-compatible con formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\-correo**  
  
    -   **Nombre común**  
  
9. Seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Pasar a través solo un determinado valor de notificación**  
  
    -   **Pasar a través de los valores de notificación que coincidan con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar a través de los valores de notificación que empiezan con un valor específico**  
![Crear regla](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. Haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para crear una regla para emitir AD FS 1. *x* notificación de Id. de nombre usando una plantilla de regla de notificación entrante de la transformación en Windows Server 2012 R2  
  
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
  
7.  En **tipo de notificación entrante**, seleccione el tipo de notificación entrante que desea transformar en la lista.  
  
8.  En **tipo de notificación saliente**, seleccione **Id. de nombre** en la lista.  
  
9. En **formato de Id. de nombre saliente**, seleccione uno de AD FS 1. *x*\-compatible con formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\-correo**  
  
    -   **Nombre común**  
  
10. Seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**  
  
    -   **Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico**  
![Crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Haga clic en **finalizar**y, a continuación, haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Creación de reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El rol de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
