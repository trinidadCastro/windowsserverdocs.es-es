---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: "Crear una regla para permitir o denegar a los usuarios en función de una notificación entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afcbb8c7a08a84eda2a794c9565061d7d61d470b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Crear una regla para permitir o denegar a los usuarios en función de una notificación entrante 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En Windows Server 2016, puedes usar un **directiva de Control de acceso** para crear una regla que permitirá de denegar a los usuarios en función de una notificación entrante.  En Windows Server 2012 R2, con la **permitir o denegar a los usuarios en función de una notificación entrante** plantilla de la regla en los servicios de federación de Active Directory \(AD FS\), puedes crear una regla de autorización que se conceder o denegar el acceso de usuario para el usuario de confianza en función del tipo y el valor de una notificación entrante. 

Por ejemplo, puedes usar esto para crear una regla que permita sólo los usuarios que tienen un grupo de notificación con un valor de administradores de dominio para acceder a la parte del usuario de confianza. Si quieres permitir que todos los usuarios para acceder a la parte del usuario de confianza, usa el **permitir que todos los usuarios** directiva de Control de acceso o la **permitir que todos los usuarios** plantilla regla según la versión de Windows Server. Los usuarios que tienen permiso para acceder a la parte del usuario de confianza de los servicios de federación de todavía se pueden denegar servicio por el usuario de confianza.  
  
Puedes usar el siguiente procedimiento para crear una regla de notificación con la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para crear una regla para permitir que los usuarios en función de una notificación entrante en Windows Server 2016
 
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **directivas de Control de acceso **. 
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Haz clic en y selecciona **Agregar directiva de Control de acceso **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. En el cuadro de nombre, escribe un nombre para la directiva, una descripción y haz clic en **agregar **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. En la **regla de Editor**, en usuarios, coloca una comprobación en **con notificaciones específicas en la solicitud** y haz clic en el subrayado **específico** en la parte inferior.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. En la **selecciona reclamaciones** de pantalla, haz clic en el **reclamaciones** botón de radio, selecciona el **tipo de notificación**, la **operador**y el **valor de notificación**, a continuación, haz clic en **Aceptar **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  En la **regla de Editor** haga clic en **Aceptar **.  En la **Agregar directiva de Control de acceso** de pantalla, haz clic en **Aceptar **.

8. En la **AD FS administración** consola árbol, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Haz clic en el **confiar confiar en las partes** que desea permitir el acceso a y selecciona **Editar directiva de Control de acceso **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. En la directiva de control de acceso, selecciona la directiva y, a continuación, haz clic en **aplicar** y **Aceptar **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para crear una regla para denegar a los usuarios en función de una notificación entrante en Windows Server 2016
 
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **directivas de Control de acceso **. 
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Haz clic en y selecciona **Agregar directiva de Control de acceso **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. En el cuadro de nombre, escribe un nombre para la directiva, una descripción y haz clic en **agregar **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. En la **regla de Editor**, asegúrate de que todo el mundo está seleccionado y, en **excepto** colocar una comprobación en **con notificaciones específicas en la solicitud** y haz clic en el subrayado **específico** en la parte inferior.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. En la **selecciona reclamaciones** de pantalla, haz clic en el **reclamaciones** botón de radio, selecciona el **tipo de notificación**, la **operador**y el **valor de notificación**, a continuación, haz clic en **Aceptar **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  En la **regla de Editor** haga clic en **Aceptar **.  En la **Agregar directiva de Control de acceso** de pantalla, haz clic en **Aceptar **.

8. En la **AD FS administración** consola árbol, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Haz clic en el **confiar confiar en las partes** que desea permitir el acceso a y selecciona **Editar directiva de Control de acceso **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. En la directiva de control de acceso, selecciona la directiva y, a continuación, haz clic en **aplicar** y **Aceptar **.
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Para crear una regla para permitir o denegar a los usuarios en función de una notificación entrante en Windows Server 2012 R2
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.    
  
2.  En el árbol de consola, en **confía en AD FS\\Trust Relationships\\Relying terceros**, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en el **las reglas de autorización de emisión** pestaña o el **las reglas de autorización de delegación** pestaña \ (en función del tipo de regla de autorización require\) y, a continuación, haz clic en **Agregar regla** para iniciar la **Agregar asistente de regla de Reclamación de autorización **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **permitir o denegar a los usuarios en función de una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escriba el nombre para mostrar para esta regla, en **tipo de notificación entrante** selecciona un tipo de notificación de la lista, en **el valor de notificación entrante** escribe un valor o haz clic en Examinar \ (si está available\) y selecciona un valor y, a continuación, selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Permitir el acceso a los usuarios con esta notificación entrante**  
  
    -   **Denegar el acceso a los usuarios con esta notificación entrante**  
![Crear la regla](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Haz clic en **finalizar**.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
