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
ms.openlocfilehash: d057c943b9c14b74b44472d446625b60f5ad9d22
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865963"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Crear una regla para permitir o denegar usuarios según una notificación entrante 


En Windows Server 2016, puede usar una **Directiva de Access Control** para crear una regla que permita o deniegue a los usuarios en función de una demanda entrante.  En Windows Server 2012 R2, el uso de la plantilla de reglas **permitir o denegar a los usuarios en función de una notificaciones entrantes** en servicios de Federación de Active Directory (AD FS) \(AD FS\), puede crear una regla de autorización que concederá o denegará el acceso del usuario al el usuario de confianza se basa en el tipo y el valor de una demanda entrante. 

Por ejemplo, puede usar esto para crear una regla que permita que solo los usuarios que tienen un grupo de notificaciones con un valor de Admins. del dominio tengan acceso al usuario de confianza. Si desea permitir que todos los usuarios accedan al usuario de confianza, use la directiva **permitir todos** los Access Control o la plantilla de regla **permitir a todos los usuarios** , en función de su versión de Windows Server. Aún así, es posible que los usuarios que tienen permiso para acceder al usuario de confianza por parte del Servicio de federación tengan el acceso denegado por parte del usuario de confianza.  
  
Puede usar el procedimiento siguiente para crear una regla de notificaciones con el complemento\-de administración de AD FS en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para crear una regla que permita a los usuarios basarse en una demanda entrante en Windows Server 2016
 
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en **directivas de Access Control**. 
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Haga clic con el botón derecho y seleccione **Agregar Directiva de Access Control**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. En el cuadro Nombre, escriba un nombre para la Directiva, una descripción y haga clic en **Agregar**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. En el **Editor de reglas**, en usuarios, coloque una inserción en **el repositorio con notificaciones específicas en la solicitud** y haga clic en el subrayado **específico** en la parte inferior.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. En la pantalla **seleccionar notificaciones** , haga clic en el botón de opción **notificaciones** , seleccione el **tipo de notificación**, el **operador**y el **valor de notificación** y, a continuación, haga clic en **Aceptar**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  En el **Editor de reglas** , haga clic en **Aceptar**.  En la pantalla **Agregar Directiva de Access Control** , haga clic en **Aceptar**.

8. En el árbol de la consola de **Administración de AD FS** , en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Haz clic con el botón derecho en la relación de confianza para usuario **autenticado** a la que deseas permitir el acceso y selecciona **editar Directiva de Access Control**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. En la Directiva de control de acceso, seleccione la Directiva y haga clic en **aplicar** y en **Aceptar**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para crear una regla para denegar a los usuarios en función de una demanda entrante en Windows Server 2016
 
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en **directivas de Access Control**. 
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Haga clic con el botón derecho y seleccione **Agregar Directiva de Access Control**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. En el cuadro Nombre, escriba un nombre para la Directiva, una descripción y haga clic en **Agregar**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. En el **Editor de reglas**, asegúrese de que todos los usuarios están seleccionados y, en **excepto** , coloque una inserción en **el repositorio con notificaciones específicas en la solicitud** y haga clic en el subrayado **específico** en la parte inferior.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. En la pantalla **seleccionar notificaciones** , haga clic en el botón de opción **notificaciones** , seleccione el **tipo de notificación**, el **operador**y el **valor de notificación** y, a continuación, haga clic en **Aceptar**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  En el **Editor de reglas** , haga clic en **Aceptar**.  En la pantalla **Agregar Directiva de Access Control** , haga clic en **Aceptar**.

8. En el árbol de la consola de **Administración de AD FS** , en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Haz clic con el botón derecho en la relación de confianza para usuario **autenticado** a la que deseas permitir el acceso y selecciona **editar Directiva de Access Control**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. En la Directiva de control de acceso, seleccione la Directiva y haga clic en **aplicar** y en **Aceptar**.
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Para crear una regla para permitir o denegar a los usuarios en función de una demanda entrante en Windows Server 2012 R2
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.    
  
2.  En el árbol de consola, **en\\AD FS relaciones\\de confianza relaciones de confianza para usuario autenticado**, haga clic en una confianza concreta de la lista en la que desea crear esta regla.  
  
3.  Haga\-clic con el botón secundario en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en la pestaña reglas de autorización de \( **emisión** o en la pestaña reglas de autorización de\) **delegación** según el tipo de regla de autorización que necesite y, a continuación, haga clic en **Agregar regla.** para iniciar el **Asistente para agregar regla de notificaciones de autorización**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **permitir o denegar a los usuarios en función de una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en **tipo de notificaciones entrantes** Seleccione un tipo de demanda en la lista \(, en **valor de notificaciones entrantes** escriba un valor o haga clic en examinar si está disponible\) y seleccione un valor y, a continuación, seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Permitir el acceso a los usuarios con esta demanda entrante**  
  
    -   **Denegar el acceso a los usuarios con esta demanda entrante**  
![crear regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Haga clic en **Finalizar**  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Cuándo usar una regla de notificaciones de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
