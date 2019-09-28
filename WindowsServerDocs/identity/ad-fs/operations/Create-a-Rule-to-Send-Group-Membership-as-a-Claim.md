---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Crear una regla para enviar la pertenencia a grupos como una notificación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b8217302cc0ec5bc6972004cb2f26ffae1371614
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407588"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Crear una regla para enviar la pertenencia a grupos como una notificación

Mediante la pertenencia al grupo de envío como una plantilla de \(regla\)de notificaciones en servicios de Federación de Active Directory (AD FS) AD FS, puede crear una regla que le permita seleccionar un grupo de seguridad de Active Directory para enviarlo como una demanda. Solo se emitirá una única demanda desde esta regla, según el grupo que seleccione. Por ejemplo, puede usar esta plantilla de reglas para crear una regla que enviará una demanda de grupo con un valor de admin si el usuario es miembro del grupo de seguridad Admins. del dominio. Esta regla solo debe usarse para los usuarios del dominio de Active Directory local.  
  
Puede usar el procedimiento siguiente para crear una regla de notificaciones con el complemento\-de administración de AD FS en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia a grupos como una demanda en una relación de confianza para usuario autenticado en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Haga\-clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en el **Grupo del usuario** , haga clic en **examinar** y seleccione un grupo, en **tipo de notificaciones salientes** , seleccione el tipo de notificaciones que desee y, a continuación, en  **Tipo de notificaciones salientes** escriba un valor.
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia a grupos como una notificación en una confianza de proveedor de notificaciones en Windows Server 2016 
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Haga\-clic con el botón secundario en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en el **Grupo del usuario** , haga clic en **examinar** y seleccione un grupo, en **tipo de notificaciones salientes** , seleccione el tipo de notificaciones que desee y, a continuación, en  **Tipo de notificaciones salientes** escriba un valor. 
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar la pertenencia a grupos como una demanda en Windows Server 2012 R2 
  
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

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en el **Grupo del usuario** , haga clic en **examinar** y seleccione un grupo, en **tipo de notificaciones salientes** , seleccione el tipo de notificaciones que desee y, a continuación, en  **Tipo de notificaciones salientes** escriba un valor.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Haga clic en **Finalizar**.  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  



## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificaciones de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
