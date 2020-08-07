---
title: Administrar estaciones de usuario
description: Aprenda a administrar estaciones de usuario en Multipoint Services
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: bcbf12b3d9a9a83cab98fb342f325436e42a17a9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951870"
---
# <a name="manage-user-stations"></a>Administrar estaciones de usuario
En esta sección se describe cómo administrar las *estaciones* que forman el sistema MultiPoint Services. La administración de un sistema Multipoint Services incluye la administración de los componentes de hardware y software de Multipoint Manager. En un sistema MultiPoint Services, el escritorio es la interfaz de usuario de software que se presenta en el monitor en cada estación de usuario.

## <a name="station-status"></a>Estado de la estación
Puede ver los siguientes tipos de estado para cada escritorio en la pestaña **estaciones** . el estado incluye:

-   Los usuarios que han iniciado sesión

-   Las sesiones de usuario que están suspendidas, pero que siguen activas en el equipo

-   Las estaciones que se están usando y por qué usuario

Para más información sobre cómo ver el estado del escritorio, vea el tema [Ver estado de la conexión de usuario](View-User-Connection-Status.md).

>[!TIP]
> Puede asignar nombres descriptivos a cada estación, lo que le ayudará a identificarlas más fácilmente. Use **Identify station** (Identificar estación), que muestra el nombre de estación en la pantalla asignada.

## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>Distintas formas de cerrar la sesión de usuarios estándar del sistema MultiPoint Services
Como *usuario administrativo*, puede cerrar la sesión de Windows en cualquier momento, sin que afecte a los usuarios activos del sistema MultiPoint Services. Los *usuarios estándar* también pueden *desconectar* su sesión o *cerrar sesión* en el sistema MultiPoint Services. En caso de que esté habilitada la protección de disco, si un usuario está terminando su jornada laboral, debe asegurarse de guardar el trabajo en el equipo o en un dispositivo de almacenamiento externo para que cuando se apague el sistema MultiPoint Services, pueda recuperar su trabajo guardado al día siguiente.

Como usuario administrativo, puede que tenga que finalizar la *sesión*de un usuario estándar, en lugar de que el usuario cierre la sesión. Puede finalizar una sesión de usuario estándar de una de estas dos maneras:

-   Finalice la sesión y cierre la sesión del usuario. Para obtener más información sobre cómo finalizar la sesión de un usuario, vea el tema [finalizar una sesión de usuario](End-a-User-Session.md) .

-   Suspender al usuario para que finalice temporalmente la sesión del usuario, pero mantenga la sesión activa en la memoria del equipo del sistema Multipoint Services. El usuario suspendido se puede volver a conectar a la sesión desde la misma estación o desde otra diferente y continuar con su trabajo. Para obtener más información acerca de la suspensión de la sesión de un usuario, vea el tema [suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md) .

## <a name="set-a-station-to-automatically-log-on"></a>Configurar una estación para que inicie sesión automáticamente
Como usuario administrativo, puede configurar una o varias estaciones para que inicien sesión automáticamente cuando que se inicie el equipo que está ejecutando MultiPoint Services. Para más información sobre cómo iniciar sesión automáticamente, vea el tema [Configurar una estación para el inicio de sesión automático](Set-up-a-Station-for-Automatic-Logon.md).

## <a name="split-a-station"></a>Dividir una estación
Cualquier monitor de estación que tenga una resolución mayor de 1024x768 se puede dividir en dos estaciones. Para más información sobre cómo dividir una estación, vea el tema [Dividir una estación de usuario](Split-a-User-Station.md).

## <a name="see-also"></a>Consulte también
[Ver el estado](View-User-Connection-Status.md) 
 de conexión del usuario [Cerrar sesión o desconectar sesiones](Log-off-or-Disconnect-User-Sessions.md) 
 de usuario [Suspender y dejar activa](Suspend-and-Leave-User-Session-Active.md) 
 la sesión de usuario [Configuración de una estación para el inicio de sesión automático](Set-up-a-Station-for-Automatic-Logon.md) 
 [Finalizar una sesión](End-a-User-Session.md) 
 de usuario [División de una estación de usuario](Split-a-User-Station.md)