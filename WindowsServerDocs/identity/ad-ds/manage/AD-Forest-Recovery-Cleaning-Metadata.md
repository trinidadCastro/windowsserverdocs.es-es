---
title: "Recuperación de bosque de AD - limpieza de metadatos de controladores de dominio quitado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adfs
ms.openlocfilehash: 3027c59b58801b44d20127e6bcf62dd7319708bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Recuperación de bosque de AD - limpieza de metadatos de controladores de dominio grabable quitado 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
 Limpieza de los metadatos quita los datos de Active Directory que identifica un controlador de dominio al sistema de replicación.  
  
 Usa el siguiente procedimiento para eliminar los objetos de DC para controladores de dominio que se va a agregar a la red reinstalando AD DS.  
  
 Si estás usando la versión de equipos y usuarios de Active Directory o sitios de Active Directory y servicios que se incluyen herramientas de administración remota del servidor (RSAT), la limpieza de metadatos se realiza automáticamente cuando se elimina un objeto de DC.  
  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Eliminación de un controlador de dominio con equipos y usuarios de Active Directory  
 Al usar la versión de los usuarios de Active Directory y equipos o centro de administración de Active Directory en herramientas de administración remota de servidor (RSAT), la limpieza de metadatos se realiza automáticamente al eliminar el objeto controlador de dominio. El objeto de servidor y el objeto de equipo también se eliminarán automáticamente.  
  
 Como alternativa, también puedes usar los servicios y sitios de Active Directory en RSAT para eliminar un objeto de controlador de dominio. Si usas servicios y sitios de Active Directory, debes eliminar el objeto de servidor asociado y el objeto de configuración NTDS antes de eliminar el objeto controlador de dominio.  
  
 Para descargar RSAT:  

-   [Herramientas de administración remota del servidor para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)
  
-   [Herramientas de administración remota del servidor para Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)  

-   [Herramientas de administración remota del servidor para Windows 7 con Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=7887)  
  
-   [Herramientas de administración de servidor remoto de Microsoft para Windows Vista](https://www.microsoft.com/download/details.aspx?id=21090)  
  
 El siguiente procedimiento es el mismo para controladores de dominio que se ejecutan en Windows Server 2016, 2008, 2008 R2 o 2012. El controlador de dominio de destino de la operación de limpieza de metadatos puede ejecutar cualquier versión de Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Para eliminar un objeto de controlador de dominio con equipos y usuarios de Active Directory en RSAT  
  
1.  Haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
2.  En el árbol de consola, haz doble clic en el contenedor de dominio y, a continuación, haz doble clic en el **controladores de dominio** unidad organizativa (OU).  
3.  En el panel de detalles, haz clic en el controlador de dominio que quieras eliminar y, a continuación, haz clic en **eliminar**. 
![Eliminar](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4.  Haz clic en **Sí** para confirmar la eliminación. Selecciona el **este controlador de dominio no está conectado permanentemente y ya no se puede degradar mediante la instalación de Active Directory dominio servicios Asistente para (DCPROMO)** casilla de verificación y haz clic en **eliminar**.  
5.  Si el controlador de dominio era un servidor de catálogo global, haz clic en **Sí** confirma que la eliminación.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
  
