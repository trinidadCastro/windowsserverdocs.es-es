---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Creación de una relación de confianza para usuario autenticado que no sea para notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d8a3c8e34c9b1ca655447b52152eb9d34c466c38
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967305"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Crear una relación de confianza para usuario autenticado no compatible con notificaciones


En el complemento de administración de AD FS \- , \- las \- relaciones de confianza para usuario autenticado no compatibles con notificaciones son objetos que se crean para representar la confianza entre el servicio de Federación y una única \- aplicación basada en Web que no es compatible con notificaciones \- y a la que se tiene acceso a través del proxy de aplicación Web.

Una relación de confianza para usuario autenticado no \- \- compatible con notificaciones es una relación de confianza para usuario autenticado que consta de identificadores, nombres y reglas para la autenticación y autorización cuando se tiene acceso a la relación de confianza para usuario autenticado a través del proxy de aplicación Web. Estas \- aplicaciones basadas en Web que no se basan en notificaciones, es decir, estas aplicaciones basadas en la autenticación integrada de Windows \- , pueden tener reglas de autorización que apliquen el acceso basado en notificaciones cuando el acceso es externo a la red corporativa a través del proxy de aplicación Web.

Para agregar una nueva \- \- relación de confianza para usuario autenticado no compatible con notificaciones, use el complemento \- de administración de AD FS en, realice el procedimiento siguiente.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Para crear una relación de confianza para usuario autenticado no compatible con notificaciones manualmente
1. En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)

3.  En la página de **bienvenida** , elija **no compatible con notificaciones** y haga clic en **iniciar**.
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG)

4.  En la página **Especificar nombre para mostrar**, escriba un nombre en **Nombre para mostrar**, en **Notas** escriba una descripción de esta relación de confianza para usuario autenticado y luego haga clic en **Siguiente**.
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. En la página **Configurar identificadores**, especifica uno o varios identificadores para este usuario de confianza, haz clic en **Agregar** para agregarlos a la lista y haz clic en **Siguiente**.
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  En **Choose Access Control Policy** (Elegir directiva de control de acceso), seleccione una directiva y haga clic en **Siguiente**.  Para obtener más información acerca de las directivas de Access Control, consulte [directivas de Access Control en AD FS](Access-Control-Policies-in-AD-FS.md).
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. En la página **Ready to Add Trust** (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.
   ![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG)

8. En la página **Finalizar**, haz clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**.
![usuario de confianza](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)

## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
