---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Creación de una relación de confianza para usuario autenticado que no sea para notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cdd0b32b50f676007a6cc922bc15b95bb61323be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189671"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Creación de una confianza de usuario de confianza que no es compatible con notificaciones


En el complemento Administración de AD FS\-en, no\-notificaciones\-consciente autenticado son objetos que se crean para representar la confianza entre el servicio de federación y un único sitio web\-aplicación que no está basada en notificaciones\-tenga en cuenta que se obtiene acceso a través del Proxy de aplicación Web.  
  
Sin\-notificaciones\-conscientes de confianza es una entidad de confianza que consta de los identificadores, nombres y reglas para la autenticación y autorización cuando se tiene acceso a la relación de confianza para usuario autenticado a través del Proxy de aplicación Web. Estas web\-en función de las aplicaciones no basadas en notificaciones, en otras palabras, estos autenticación integrada de Windows\-aplicaciones basadas en, puede tener reglas de autorización que se aplican el acceso basado en notificaciones cuando el acceso externo a la red corporativa a través del Proxy de aplicación Web.  
  
Para agregar un nuevo no\-notificaciones\-compatible con confianza, utilizando el complemento Administración de AD FS\-, realice el procedimiento siguiente.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Crear manualmente un no basada en notificaciones compatible con confianza 
1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En **acciones**, haga clic en **agregar confianza**.  
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  En el **bienvenida** página, elija **notificaciones no** y haga clic en **iniciar**.  
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  En el **especificar nombre para mostrar** página, escriba un nombre en **nombre para mostrar**, en **notas** escriba una descripción para esta relación de confianza y, a continuación, haga clic en **siguiente** .  
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. En la página **Configurar identificadores**, especifica uno o varios identificadores para este usuario de confianza, haz clic en **Agregar** para agregarlos a la lista y haz clic en **Siguiente**.  
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  En el **elegir directiva de Control de acceso** seleccione una directiva y haga clic en **siguiente**.  Para obtener más información acerca de las directivas de Control de acceso, consulte [las directivas de Control de acceso en AD FS](Access-Control-Policies-in-AD-FS.md). 
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. En la página **Listo para agregar confianza** , revisa la configuración y haz clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.  
   ![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. En la página **Finalizar**, haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**.  
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
