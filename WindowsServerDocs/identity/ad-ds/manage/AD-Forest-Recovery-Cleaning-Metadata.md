---
title: 'Recuperación de bosques de AD: limpieza de metadatos de controladores de DC quitados'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: 4bf3ec5cb9495e3603c3a5a385f0ff7b65e9d8b7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962948"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Recuperación de bosques de AD: limpiar metadatos de controladores de dominio de escritura quitados

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

La limpieza de metadatos quita Active Directory datos que identifican un controlador de dominio en el sistema de replicación.  

Use el procedimiento siguiente para eliminar los objetos DC de los controladores de dominio que va a agregar de nuevo a la red reinstalando AD DS.  
  
Si usa la versión de Active Directory usuarios y equipos, o Active Directory sitios y servicios incluidos Herramientas de administración remota del servidor (RSAT), la limpieza de metadatos se realiza automáticamente al eliminar un objeto DC.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Eliminación de un controlador de dominio con usuarios y equipos de Active Directory

Cuando se usa la versión de Active Directory usuarios y equipos o Centro de administración de Active Directory en Herramientas de administración remota del servidor (RSAT), la limpieza de metadatos se realiza automáticamente al eliminar el objeto DC. El objeto de servidor y el objeto de equipo también se eliminan automáticamente.  

Como alternativa, también puede usar sitios y servicios de Active Directory en RSAT para eliminar un objeto DC. Si usa Active Directory sitios y servicios, debe eliminar el objeto de servidor asociado y el objeto de configuración NTDS antes de poder eliminar el objeto DC.  

Para obtener información sobre cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](../../../remote/remote-server-administration-tools.md).
  
El siguiente procedimiento es el mismo para los controladores de DC que ejecutan Windows Server 2016, 2012, 2008 R2 o 2008. El controlador de dominio de destino de la operación de limpieza de metadatos puede ejecutar cualquier versión de Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Para eliminar un objeto de controlador de dominio mediante usuarios y equipos de Active Directory en RSAT  
  
1. Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**.  
2. En el árbol de consola, haga doble clic en el contenedor de dominio y, a continuación, haga doble clic en la unidad organizativa (OU) **controladores de dominio** .  
3. En el panel de detalles, haga clic con el botón secundario en el controlador de dominio que desea eliminar y, a continuación, haga clic en **eliminar**.
   ![Eliminar](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Haga clic en **Sí** para confirmar la eliminación. Seleccione **este controlador de dominio está permanentemente sin conexión y ya no se puede disminuir de nivel con la casilla Asistente para instalación de Active Directory Domain Services (Dcpromo)** y haga clic en **eliminar**.  
5. Si el controlador de dominio era un servidor de catálogo global, haga clic en **sí** confirmar la eliminación.  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
