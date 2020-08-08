---
title: Cerrar sesión o desconectar sesiones de usuario
description: Obtenga información sobre cómo cerrar sesión de un usuario manualmente
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 0436f33df87da55792ba01a581c8f5c05f3500ce
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969332"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Cerrar sesión o desconectar sesiones de usuario
Los usuarios de MultiPoint Services pueden iniciar sesión y cerrar sesión de sus sesiones de escritorio al igual que harían en cualquier sesión de Windows. Los usuarios también pueden desconectar o suspender su sesión para que no se use la estación Multipoint Services, pero su sesión permanece activa en la memoria del equipo del sistema Multipoint Services.

Además, los usuarios administrativos pueden finalizar la sesión de un usuario si el usuario se ha desplazado fuera de la sesión de Multipoint Services o ha olvidado cerrar la sesión del sistema.

## <a name="logging-off-or-disconnecting-a-session"></a>Cerrar o desconectar una sesión
En la tabla siguiente se describen las distintas opciones que el usuario puede usar para cerrar, suspender o finalizar una sesión.

|||
|-|-|
|**Acción**|**Efecto**|
|Haga clic en **Inicio**, haga clic en configuración, haga clic en el nombre de usuario (esquina superior derecha) y, a continuación, haga clic en **Cerrar sesión**.|La sesión finaliza y la estación estará disponible para que otro usuario inicie sesión.|
|Haga clic en **Inicio**, **Configuración**, Energía y **Desconectar**.|La sesión se desconecta y se guarda en la memoria del equipo. La estación estará disponible para que el mismo usuario u otro diferente inicien sesión.|
|Haga clic en **Inicio**, seleccione Configuración, haga clic en el nombre de usuario (esquina superior derecha) y, a continuación, haga clic en **bloquear** .|La estación se bloquea y la sesión se guarda en la memoria del equipo.|

## <a name="suspending-or-ending-a-users-session"></a>Suspender o finalizar una sesión de usuario
En la tabla siguiente se describen las distintas opciones que usted, como usuario administrativo, puede usar para desconectar o finalizar la sesión de un usuario.

|||
|-|-|
|**Acción**|**Efecto**|
|**Suspender:** En Multipoint Manager, use la pestaña **estaciones** para suspender la sesión del usuario. Para más información, vea el tema [Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md).|La sesión del usuario finaliza y se conserva en la memoria del equipo. La estación estará disponible para que el mismo usuario u otro diferente inicien sesión. El usuario puede iniciar sesión en la misma estación o en otra diferente y continuar con su trabajo.|
|**Fin:** En Multipoint Manager, use la pestaña **estaciones** para finalizar la sesión del usuario. También puede finalizar todas las sesiones de usuario en la pestaña **estaciones** . Para obtener más información, vea el tema [finalizar una sesión de usuario](End-a-User-Session.md) .|La sesión del usuario finaliza y la estación está disponible para que el usuario inicie sesión. La sesión del usuario ya no se muestra en la pestaña **estaciones** y no se encuentra en la memoria del equipo.|

## <a name="see-also"></a>Consulte también
[Suspender y dejar activa](Suspend-and-Leave-User-Session-Active.md) 
 la sesión de usuario [Finalizar una sesión](End-a-User-Session.md) 
 de usuario [Administrar escritorios](manage-user-desktops-using-multipoint-dashboard.md) 
 de usuario [Cerrar sesiones de usuario](Log-Off-User-Sessions.md)