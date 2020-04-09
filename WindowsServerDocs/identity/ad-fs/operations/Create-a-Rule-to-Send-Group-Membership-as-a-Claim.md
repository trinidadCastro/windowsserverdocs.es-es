---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Crear una regla para enviar la pertenencia a grupos como una notificación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5777a3310776115f02395df365352be94a89928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816668"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Crear una regla para enviar la pertenencia a grupos como una notificación

Mediante la pertenencia al grupo de envío como una plantilla de regla de notificaciones en Servicios de federación de Active Directory (AD FS) \(AD FS\), puede crear una regla que le permita seleccionar un grupo de seguridad de Active Directory para enviarlo como una demanda. Solo se emitirá una única demanda desde esta regla, según el grupo que seleccione. Por ejemplo, puede usar esta plantilla de reglas para crear una regla que enviará una demanda de grupo con un valor de admin si el usuario es miembro del grupo de seguridad Admins. del dominio. Esta regla solo debe usarse para los usuarios del dominio de Active Directory local.  
  
Puede usar el siguiente procedimiento para crear una regla de notificaciones con el complemento de administración de AD FS\-en.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia a grupos como una demanda en una relación de confianza para usuario autenticado en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   En la página **configurar regla** , en nombre de la **regla de notificaciones** escriba el nombre para mostrar de esta regla, en **grupo de usuarios** , haga clic en **examinar** y seleccione un grupo, en **tipo de notificaciones salientes** , seleccione el tipo de notificaciones que desee y, a continuación, en **tipo de notificaciones salientes** escriba un valor.
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia a grupos como una notificación en una confianza de proveedor de notificaciones en Windows Server 2016 
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   En la página **configurar regla** , en nombre de la **regla de notificaciones** escriba el nombre para mostrar de esta regla, en **grupo de usuarios** , haga clic en **examinar** y seleccione un grupo, en **tipo de notificaciones salientes** , seleccione el tipo de notificaciones que desee y, a continuación, en **tipo de notificaciones salientes** escriba un valor. 
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar la pertenencia a grupos como una demanda en Windows Server 2012 R2 
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas para usuario autenticado**y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.  
  
3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, en función de la confianza que esté editando y el conjunto de reglas en el que desee crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **Enviar pertenencia a grupos como una demanda** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** escriba el nombre para mostrar de esta regla, en **grupo de usuarios** , haga clic en **examinar** y seleccione un grupo, en **tipo de notificaciones salientes** , seleccione el tipo de notificaciones que desee y, a continuación, en **tipo de notificaciones salientes** escriba un valor.  
![crear regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Haga clic en **Finalizar**.  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  



## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: creación de reglas de notificaciones para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: creación de reglas de notificación para una confianza de proveedor de notificaciones](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificaciones de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
