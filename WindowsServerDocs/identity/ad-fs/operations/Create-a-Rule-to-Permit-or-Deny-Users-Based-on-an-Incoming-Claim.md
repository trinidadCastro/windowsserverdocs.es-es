---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: Crear una regla para permitir o denegar usuarios según una notificación entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 167e43d49c08d0e39549bf46888118f985e3876d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863776"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Crear una regla para permitir o denegar usuarios según una notificación entrante 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En Windows Server 2016, puede usar un **directiva de Control de acceso** para crear una regla que permitirá o denegará a los usuarios según una notificación entrante.  En Windows Server 2012 R2, mediante el **permitir o denegar usuarios según una notificación entrante** plantilla de regla en Active Directory Federation Services \(AD FS\), puede crear una regla de autorización que se concederá o denegar el acceso de usuario al usuario de confianza según el tipo y el valor de una notificación entrante. 

Por ejemplo, puede usar para crear una regla que permitirá a solo los usuarios que tienen un grupo de notificación con un valor de Admins. del dominio para tener acceso a la entidad de usuario de confianza. Si desea permitir que todos los usuarios para tener acceso a la entidad de usuario de confianza, use el **permitir todo el mundo** directiva de Control de acceso o el **permitir todos los usuarios** plantilla de regla según la versión de Windows Server. Aún así, es posible que los usuarios que tienen permiso para acceder al usuario de confianza por parte del Servicio de federación tengan el acceso denegado por parte del usuario de confianza.  
  
Puede usar el procedimiento siguiente para crear una regla de notificación con el complemento Administración de AD FS\-en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para crear una regla para permitir a los usuarios según una notificación entrante en Windows Server 2016
 
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **las directivas de Control de acceso**. 
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Haga clic en y seleccione **Agregar directiva de Control de acceso**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. En el cuadro Nombre, escriba un nombre para la directiva, una descripción y haga clic en **agregar**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. En el **Editor de reglas**, en usuarios, coloque una marca de verificación **con notificaciones específicas en la solicitud** y haga clic en el texto subrayado **específico** en la parte inferior.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. En el **seleccionar notificaciones** pantalla, haga clic en el **notificaciones** botón de radio, seleccione el **tipo de notificación**, el **operador**y el  **Valor de notificación** , a continuación, haga clic en **Aceptar**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  En el **Editor de reglas** haga clic en **Aceptar**.  En el **Agregar directiva de Control de acceso** pantalla, haga clic en **Aceptar**.

8. En el **administración de AD FS** árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Haga clic en el **confianza** que desea permitir el acceso a y seleccione **Editar directiva de Control de acceso**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. En la directiva de control de acceso, seleccione la directiva y, a continuación, haga clic en **aplicar** y **Aceptar**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para crear una regla para denegar usuarios según una notificación entrante en Windows Server 2016
 
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **las directivas de Control de acceso**. 
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Haga clic en y seleccione **Agregar directiva de Control de acceso**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. En el cuadro Nombre, escriba un nombre para la directiva, una descripción y haga clic en **agregar**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. En el **Editor de reglas**, asegúrese de que todo el mundo está seleccionado y, en **excepto** colocar una marca de verificación **con notificaciones específicas en la solicitud** y haga clic en el texto subrayado  **específica** en la parte inferior.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. En el **seleccionar notificaciones** pantalla, haga clic en el **notificaciones** botón de radio, seleccione el **tipo de notificación**, el **operador**y el  **Valor de notificación** , a continuación, haga clic en **Aceptar**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  En el **Editor de reglas** haga clic en **Aceptar**.  En el **Agregar directiva de Control de acceso** pantalla, haga clic en **Aceptar**.

8. En el **administración de AD FS** árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Haga clic en el **confianza** que desea permitir el acceso a y seleccione **Editar directiva de Control de acceso**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. En la directiva de control de acceso, seleccione la directiva y, a continuación, haga clic en **aplicar** y **Aceptar**.
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Para crear una regla para permitir o denegar usuarios según una notificación entrante en Windows Server 2012 R2
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.    
  
2.  En el árbol de consola, bajo **AD FS\\relaciones de confianza\\autenticado**, haga clic en una relación de confianza específico en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en el **reglas de autorización de emisión** ficha o el **las reglas de autorización de delegación** ficha \(según el tipo de regla de autorización que requiere\)y, a continuación, haga clic en **Agregar regla** para iniciar el **Agregar asistente de regla de notificación de autorización**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **permitir o denegar usuarios según una notificación entrante** en la lista y, a continuación, haga clic en  **Siguiente**.  
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **tipo de notificación entrante** seleccionar un tipo de notificación en la lista, en  **Valor de notificación entrante** escribir un valor o haga clic en Examinar \(si está disponible\) y seleccione un valor y, a continuación, seleccione una de las opciones siguientes, según las necesidades de su organización:  
  
    -   **Permitir el acceso a los usuarios con esta notificación entrante**  
  
    -   **Denegar el acceso a los usuarios con esta notificación entrante**  
![Crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Haga clic en **Finalizar**.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Creación de reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El rol de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
