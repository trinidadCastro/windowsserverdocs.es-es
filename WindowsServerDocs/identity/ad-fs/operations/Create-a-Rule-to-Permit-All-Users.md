---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Crear una regla para permitir a todos los usuarios
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1356218c5f9f47073f007286e8acfdf4c3608b73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407627"
---
# <a name="create-a-rule-to-permit-all-users"></a>Crear una regla para permitir a todos los usuarios

En Windows Server 2016, puede usar una **Directiva de Access Control** para crear una regla que dará acceso a todos los usuarios a un usuario de confianza.  En Windows Server 2012 R2, si se usa la plantilla de regla **permitir a todos los usuarios** de Servicios de federación de Active Directory (AD FS) \(AD FS\), puede crear una regla de autorización que concederá acceso a todos los usuarios al usuario de confianza. 

Puede usar reglas de autorización adicionales para restringir aún más el acceso. Aún así, es posible que los usuarios que tienen permiso para acceder al usuario de confianza por parte del Servicio de federación tengan el acceso denegado por parte del usuario de confianza.  
  
Puede usar los procedimientos siguientes para crear una regla de notificaciones con el complemento de administración de AD FS\-en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Para crear una regla que permita a todos los usuarios de Windows Server 2016

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Haz clic con el botón derecho en la relación de confianza para usuario **autenticado** a la que deseas permitir el acceso y selecciona **editar Directiva de Access Control**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. En la Directiva de control de acceso, seleccione **permitir todos** y, a continuación, haga clic en **aplicar** y en **Aceptar**.
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Para crear una regla que permita a todos los usuarios de Windows Server 2012 R2 
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS\\relaciones de confianza\\confianzas para usuario autenticado**, haga clic en una confianza específica en la lista en la que desea crear esta regla.  

3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en la pestaña **reglas de autorización de emisión** o en la pestaña reglas de autorización de **delegación** \(según el tipo de regla de autorización que necesite\)y, a continuación, haga clic en **Agregar regla** para iniciar el **Asistente para agregar regla de notificaciones de autorización**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **permitir todos los usuarios** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  En la página **configurar regla** , haga clic en **Finalizar**.  
  
7.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: creación de reglas de notificaciones para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Cuándo usar una regla de notificaciones de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
