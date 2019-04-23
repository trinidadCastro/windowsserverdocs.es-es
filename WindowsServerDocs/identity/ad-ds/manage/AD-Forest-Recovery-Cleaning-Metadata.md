---
title: Recuperación de bosques de AD - limpieza de metadatos de controladores de dominio eliminado
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b71cab51a362a96ab6071e5eed3cf31c4421041c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843046"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Recuperación de bosques de AD - limpieza de metadatos de controladores de dominio grabables quitados

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Limpieza de metadatos quita los datos de Active Directory que identifica un controlador de dominio para el sistema de replicación.  

Utilice el procedimiento siguiente para eliminar los objetos de controlador de dominio para los controladores de dominio que va a agregar a la red mediante la reinstalación de AD DS.  
  
Si está utilizando la versión de los equipos y usuarios de Active Directory o sitios de Active Directory y servicios que se incluyen las herramientas de administración remota de servidor (RSAT), la limpieza de metadatos se realiza automáticamente cuando se elimina un objeto de controlador de dominio.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Eliminación de un controlador de dominio con equipos y usuarios de Active Directory

Cuando se usa la versión de usuarios de Active Directory y los equipos o centro de administración de Active Directory en herramientas de administración remota de servidor (RSAT), la limpieza de metadatos se realiza automáticamente cuando se elimina el objeto de controlador de dominio. El objeto de servidor y el objeto de equipo también se eliminan automáticamente.  

Como alternativa, puede usar también los servicios y sitios de Active Directory en RSAT para eliminar un objeto de controlador de dominio. Si usa servicios y sitios de Active Directory, debe eliminar el objeto de servidor asociado y el objeto de configuración NTDS para poder eliminar el objeto de controlador de dominio.  

Para obtener información acerca de cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
  
El siguiente procedimiento es el mismo para los controladores de dominio que se ejecutan en Windows Server 2016, 2012, 2008 R2 o 2008. El controlador de dominio de destino de la operación de limpieza de metadatos puede ejecutar cualquier versión de Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Para eliminar un objeto de controlador de dominio con equipos y usuarios de Active Directory en RSAT  
  
1. Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**.  
2. En el árbol de consola, haga doble clic en el contenedor de dominio y, a continuación, haga doble clic en el **controladores de dominio** unidad organizativa (OU).  
3. En el panel de detalles, haga clic en el controlador de dominio que desea eliminar y, a continuación, haga clic en **eliminar**.
   ![Eliminar](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Haga clic en **Sí** para confirmar la eliminación. Seleccione el **este controlador de dominio está permanentemente sin conexión y no puede degradarse con el Active Directory Domain Services instalación asistente (DCPROMO)** casilla de verificación y haga clic en **eliminar**.  
5. Si el controlador de dominio era un servidor de catálogo global, haga clic en **Sí** que confirme la eliminación.  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
