---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Crear una regla para permitir que todos los usuarios
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-all-users"></a>Crear una regla para permitir que todos los usuarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En Windows Server 2016, puedes usar un **directiva de Control de acceso** para crear una regla que proporcionará todos los usuarios acceso a un usuario de confianza.  En Windows Server 2012 R2, con la **permitir que todos los usuarios** plantilla de la regla en los servicios de federación de Active Directory \(AD FS\), puedes crear una regla de autorización que proporcionará a todos los usuarios acceso a la parte del usuario de confianza. 

Puedes usar reglas de autorización adicional para restringir aún más el acceso. Los usuarios que tienen permiso para acceder a la parte del usuario de confianza de los servicios de federación de todavía se pueden denegar servicio por el usuario de confianza.  
  
Puedes usar los siguientes procedimientos para crear una regla de notificación con la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Para crear una regla para permitir que todos los usuarios de Windows Server 2016

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Haz clic en el **confiar confiar en las partes** que desea permitir el acceso a y selecciona **Editar directiva de Control de acceso **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. En el acceso de control directiva selecciona **permitir que todos** y, a continuación, haz clic en **aplicar** y **Aceptar **.
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Para crear una regla para permitir que todos los usuarios de Windows Server 2012 R2 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **confía en AD FS\\Trust Relationships\\Relying terceros**, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  

3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en el **las reglas de autorización de emisión** pestaña o el **las reglas de autorización de delegación** pestaña \ (en función del tipo de regla de autorización require\) y, a continuación, haz clic en **Agregar regla** para iniciar la **Agregar asistente de regla de Reclamación de autorización **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **permitir que todos los usuarios** de la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  En la **configurar regla** página, haz clic en **finalizar **.  
  
7.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
