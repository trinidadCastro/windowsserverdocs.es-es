---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Creación de una relación de confianza para usuario autenticado que no sea para notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0ea877170a07db6abe9ac82e72d1722600ec933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358111"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Crear una relación de confianza para usuario autenticado no compatible con notificaciones


En el AD FS Management Snap @ no__t-0in, no @ no__t-1claims @ no__t-2aware las relaciones de confianza para usuario autenticado son objetos que se crean para representar la confianza entre el servicio de Federación y una única aplicación web @ no__t-3based que no es Claims @ no__t-4aware y que se tiene acceso a través del proxy de aplicación Web.  
  
Una relación de confianza para usuario autenticado que no es @ no__t-0claims @ no__t es una relación de confianza para usuario autenticado que consta de identificadores, nombres y reglas para la autenticación y autorización cuando se tiene acceso a la relación de confianza para usuario autenticado a través del proxy de aplicación Web. Estas aplicaciones web @ no__t-0based que no se basan en las notificaciones, es decir, estas aplicaciones integradas de autenticación de Windows @ no__t-1based, pueden tener reglas de autorización que aplican el acceso basado en notificaciones cuando el acceso es externo al red corporativa a través del proxy de aplicación Web.  
  
Para agregar una nueva relación de confianza para usuario autenticado que no sea @ no__t-0claims @ no__t-1aware, use el complemento de administración de AD FS @ no__t-2in, realice el procedimiento siguiente.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Para crear una relación de confianza para usuario autenticado no compatible con notificaciones manualmente 
1. En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.  
![relying Party @ no__t-1   

3.  En la página de **bienvenida** , elija **no compatible con notificaciones** y haga clic en **iniciar**.  
![relying Party @ no__t-1 
  
4.  En la **página especificar nombre para mostrar** , escriba un nombre en **nombre para mostrar**, en **notas** escriba una descripción para esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.  
![relying Party @ no__t-1

5. En la página **Configurar identificadores**, especifica uno o varios identificadores para este usuario de confianza, haz clic en **Agregar** para agregarlos a la lista y haz clic en **Siguiente**.  
![relying Party @ no__t-1

6.  En la **Directiva elegir Access Control** , seleccione una directiva y haga clic en **siguiente**.  Para obtener más información acerca de las directivas de Access Control, consulte [directivas de Access Control en AD FS](Access-Control-Policies-in-AD-FS.md). 
![relying Party @ no__t-1

7. En la página **Listo para agregar confianza** , revisa la configuración y haz clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.  
   ![relying Party @ no__t-1 

8. En la página **Finalizar**, haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**.  
![relying Party @ no__t-1  
  
## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
