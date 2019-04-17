---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Crear un no reclamaciones con reconocimiento de usuario de confianza
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Crear una confianza de terceros no notificaciones confiar

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En la en snap\, non\ claims\-reconocimiento confiar confianzas de terceros de administración de AD FS son objetos que se crean para representar la confianza entre el servicio de federación y una aplicación basada en web\ único que no es compatible con claims\ y que se tiene acceso a través del Proxy de aplicación Web.  
  
Una cuenta non\ claims\ confianza terceros confianza es usuario de confianza terceros que consta de reglas para la autenticación y autorización, nombres y los identificadores cuando se accede a la confianza de terceros de confianza a través del Proxy de aplicación Web. Estas aplicaciones basadas en web\ que no dependen de reclamaciones, en otras palabras, estas aplicaciones basadas en Authentication\ integrado de Windows, puede tener las reglas de autorización que aplicar el acceso a la que se basa en notificaciones cuando el acceso no pertenece a la red corporativa a través del Proxy de aplicación Web.  
  
Para agregar una nueva cuenta non\ claims\ confianza terceros confianza, mediante la administración de AD FS en snap\, realizar el siguiente procedimiento.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Crear una no reclamaciones cuenta confiar terceros confianza manualmente 
1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En **acciones**, haz clic en **agregar confiar terceros de confianza **.  
![Usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  En la **bienvenida** página, elige **no reclamaciones cuenta** y haz clic en **inicio**.  
![Usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  En la **especificar nombre para mostrar**, escriba un nombre en **nombre para mostrar**, en **notas** escribe una descripción para esta confianza de terceros de confianza y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. En la **configurar identificadores** página, especifica uno o más identificadores para este usuario de confianza, haz clic en **agregar** para agregarlos a la lista y, a continuación, haz clic en **siguiente**.  
![Usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  En la **elegir la directiva de Control de acceso** selecciona una directiva y haz clic en **siguiente**.  Para obtener más información acerca de las directivas de Control de acceso, consulta [directivas de Control de acceso de AD FS](Access-Control-Policies-in-AD-FS.md). 
![Usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. En la **listo para agregar confianza** página, revisa la configuración y, a continuación, haz clic en **siguiente** para el usuario de confianza de guardar información de confianza.  
   ![Usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. En la **finalizar** página, haz clic en **cerrar**. Esta acción se muestra automáticamente el **editar reglas de notificación** cuadro de diálogo.  
![Usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Consulta también  
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md) 
