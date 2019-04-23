---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Crear una regla para enviar la pertenencia a grupos como una notificación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 96ab653393fbc5f0a4306db53f84c2d9ba6c7f5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847456"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Crear una regla para enviar la pertenencia a grupos como una notificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Mediante la pertenencia al grupo de envío como una plantilla de regla de notificación en los servicios de federación de Active Directory \(AD FS\), puede crear una regla que hará posible para seleccionar un grupo de seguridad de Active Directory que se envían como una notificación. Se va a emitir solo una notificación única de esta regla, según el grupo que seleccione. Por ejemplo, puede usar esta plantilla de regla para crear una regla que va a enviar una notificación de grupo con un valor de administrador si el usuario es miembro del grupo de seguridad Admins. del dominio. Esta regla se debe usar solo para los usuarios del dominio de Active Directory local.  
  
Puede usar el procedimiento siguiente para crear una regla de notificación con el complemento Administración de AD FS\-en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia a grupos como una notificación en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar pertenencia a grupo como notificación** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **grupo del usuario** haga clic en **examinar** y seleccione un grupo, bajo **tipo de notificación saliente** seleccione el tipo de notificación deseada y, a continuación, en **tipo de notificación saliente** escriba un valor.
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia a grupos como una notificación en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar pertenencia a grupo como notificación** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **grupo del usuario** haga clic en **examinar** y seleccione un grupo, bajo **tipo de notificación saliente** seleccione el tipo de notificación deseada y, a continuación, en **tipo de notificación saliente** escriba un valor. 
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar la pertenencia a grupos como una notificación en Windows Server 2012 R2 
  
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

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **grupo del usuario** haga clic en **examinar** y seleccione un grupo, bajo **tipo de notificación saliente** seleccione el tipo de notificación deseada y, a continuación, en **tipo de notificación saliente** escriba un valor.  
![Crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Haga clic en **Finalizar**.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  



## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Creación de reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El rol de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
