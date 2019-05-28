---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Crear una regla para permitir a todos los usuarios
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: abb00e14dd0b3ce7b06efba816fbd7452e7bf0f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189409"
---
# <a name="create-a-rule-to-permit-all-users"></a>Crear una regla para permitir a todos los usuarios

En Windows Server 2016, puede usar un **directiva de Control de acceso** para crear una regla que proporcionará a todos los usuarios acceso a un usuario de confianza.  En Windows Server 2012 R2, mediante el **permitir todos los usuarios** plantilla de regla en Active Directory Federation Services \(AD FS\), puede crear una regla de autorización que proporcionará a todos los usuarios acceso para el usuario de confianza la entidad. 

Puede usar reglas de autorización adicionales para restringir aún más el acceso. Aún así, es posible que los usuarios que tienen permiso para acceder al usuario de confianza por parte del Servicio de federación tengan el acceso denegado por parte del usuario de confianza.  
  
Puede usar los procedimientos siguientes para crear una regla de notificación con el complemento Administración de AD FS\-en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Para crear una regla para permitir que todos los usuarios en Windows Server 2016

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Haga clic en el **confianza** que desea permitir el acceso a y seleccione **Editar directiva de Control de acceso**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. En el acceso de control seleccione Directiva **permitir todos los usuarios** y, a continuación, haga clic en **aplicar** y **Aceptar**.
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Para crear una regla para permitir que todos los usuarios en Windows Server 2012 R2 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS\\relaciones de confianza\\autenticado**, haga clic en una relación de confianza específico en la lista donde desea crear esta regla.  

3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en el **reglas de autorización de emisión** ficha o el **las reglas de autorización de delegación** ficha \(según el tipo de regla de autorización que requiere\)y, a continuación, haga clic en **Agregar regla** para iniciar el **Agregar asistente de regla de notificación de autorización**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **permitir todos los usuarios** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  En el **configurar regla** página, haga clic en **finalizar**.  
  
7.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
