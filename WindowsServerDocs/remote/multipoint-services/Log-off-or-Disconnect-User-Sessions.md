---
title: Cerrar sesión o desconectar sesiones de usuario
description: Obtenga información sobre cómo cerrar manualmente un usuario
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 518e9dc9ba9603d988a7e21e08caa29db9f04bde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854946"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Cerrar sesión o desconectar sesiones de usuario
Los usuarios de multiPoint Services pueden iniciar sesión y cerrar sus sesiones de escritorio como lo harían con cualquier sesión de Windows. Los usuarios también pueden desconectarse o suspender su sesión para que no se usa la estación de MultiPoint Services, pero su sesión permanece activa en la memoria del equipo del sistema MultiPoint Services.  
  
Además, los usuarios administrativos pueden finalizar una sesión de usuario si el usuario ha salido de su sesión de MultiPoint Services o ha olvidado cerrar la sesión en el sistema.  
  
## <a name="logging-off-or-disconnecting-a-session"></a>Cerrar o desconectar una sesión  
En la tabla siguiente se describen las distintas opciones que el usuario puede usar para cerrar, suspender o finalizar una sesión.  
  
|||  
|-|-|  
|**Acción**|**Efecto**|  
|Haga clic en **iniciar**, haga clic en configuración, haga clic en el nombre de usuario (esquina superior derecha) y, a continuación, haga clic en **cerrar sesión**.|La sesión finaliza y la estación estará disponible para que otro usuario inicie sesión.|  
|Haga clic en **Inicio**, **Configuración**, Energía y **Desconectar**.|La sesión se desconecta y se guarda en la memoria del equipo. La estación estará disponible para que el mismo usuario u otro diferente inicien sesión.|  
|Haga clic en **iniciar**, haga clic en configuración, haga clic en el nombre de usuario (esquina superior derecha) y, a continuación, haga clic en **bloqueo**|La estación se bloquea y la sesión se guarda en la memoria del equipo.|  
  
## <a name="suspending-or-ending-a-users-session"></a>Suspender o finalizar una sesión de usuario  
En esta tabla se describen las distintas opciones que el usuario administrativo puede usar para desconectar o finalizar una sesión de usuario.  
  
|||  
|-|-|  
|**Acción**|**Efecto**|  
|**Suspender:** En el Administrador de MultiPoint, utilice el **estaciones** tab para suspender la sesión del usuario. Para más información, vea el tema [Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md).|La sesión del usuario finaliza y se guarda en la memoria del equipo. La estación estará disponible para que el mismo usuario u otro diferente inicien sesión. El usuario puede iniciar sesión en la misma estación o en otra diferente y continuar con su trabajo.|  
|**Fin:** En el Administrador de MultiPoint, utilice el **estaciones** tab para finalizar la sesión del usuario. También puede finalizar todas las sesiones de usuario en la pestaña **Estaciones**. Para más información, vea el tema [Finalizar una sesión de usuario](End-a-User-Session.md).|La sesión del usuario finaliza y la estación estará disponible para que otro usuario inicie sesión. La sesión del usuario ya no aparece en la pestaña **Estaciones** y no está en la memoria del equipo.|  
  
## <a name="see-also"></a>Vea también  
[Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md)  
[Finalizar una sesión de usuario](End-a-User-Session.md)  
[Administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md)  
[Cerrar las sesiones de usuario](Log-Off-User-Sessions.md)    