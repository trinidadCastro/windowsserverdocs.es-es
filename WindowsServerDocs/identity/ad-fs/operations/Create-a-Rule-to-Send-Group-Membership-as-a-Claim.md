---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: "Crear una regla para enviar pertenencia a grupos como una notificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d4fb03de9de2dcca36b18ce089db11ed599de820
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Crear una regla para enviar pertenencia a grupos como una notificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Usar la pertenencia al grupo de enviar como una plantilla de regla de notificación en los servicios de federación de Active Directory \(AD FS\), puedes crear una regla que permite seleccionar un grupo de seguridad de Active Directory para enviar como una reclamación. Solo una reclamación solo se emitirá de esta regla, basada en el grupo que selecciones. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que enviará una notificación de grupo con un valor de administrador si el usuario es miembro del grupo de seguridad Administradores de dominio. Esta regla debe usarse solo para los usuarios del dominio de Active Directory local.  
  
Puedes usar el siguiente procedimiento para crear una regla de notificación con la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia al grupo como una notificación en un confiar terceros de confianza en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar pertenencia a grupos como notificación** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   En la **configurar regla** página en **nombre de regla de notificación** escriba el nombre para mostrar para esta regla, en **grupo del usuario** haga clic en **examinar** y selecciona un grupo, en **saliente de tipo de notificación** seleccione el tipo de notificación deseado y, a continuación, en **el tipo de notificación saliente** escribe un valor.
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.
  
## <a name="to-create-a-rule-to-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar la pertenencia al grupo como una notificación en un proveedor de notificaciones de confianza en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar pertenencia a grupos como notificación** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   En la **configurar regla** página en **nombre de regla de notificación** escriba el nombre para mostrar para esta regla, en **grupo del usuario** haga clic en **examinar** y selecciona un grupo, en **saliente de tipo de notificación** seleccione el tipo de notificación deseado y, a continuación, en **el tipo de notificación saliente** escribe un valor. 
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar la pertenencia al grupo como una notificación en Windows Server 2012 R2 
  
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

6.  En la **configurar regla** página en **nombre de regla de notificación** escriba el nombre para mostrar para esta regla, en **grupo del usuario** haga clic en **examinar** y selecciona un grupo, en **saliente de tipo de notificación** seleccione el tipo de notificación deseado y, a continuación, en **el tipo de notificación saliente** escribe un valor.  
![Crear la regla](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Haz clic en **finalizar**.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  



## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
