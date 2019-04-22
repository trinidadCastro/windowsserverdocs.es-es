---
title: Suspender y dejar activa la sesión de usuario
description: Obtenga información sobre cómo suspender un usuario desde una sesión de MultiPoint sin desconectarlos
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: cc4310e6f7609464cf037b750bec6e5e805e0b26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815226"
---
# <a name="suspend-and-leave-user-session-active"></a>Suspender y dejar activa la sesión de usuario
Puede desconectar o suspender los usuarios del sistema MultiPoint Services cuando no desea finalizar las sesiones de los usuarios. Un usuario también puede desconectar la sesión, en lugar de ser el usuario administrativo el que la desconecte. Mientras una sesión de usuario está suspendida, la sesión sigue estando activa en la memoria del equipo del sistema MultiPoint Services hasta que el equipo se apague o se reinicie. En ese momento, todas las sesiones suspendidas finalizan y se perderá todo el trabajo que no se haya guardado.  
  
1.  Abra MultiPoint Manager en modo de estación y, a continuación, haga clic en el **estaciones** ficha.  
  
2.  En la columna **Equipo**, haga clic en el nombre del equipo cuyas sesiones quiere suspender.  
  
3.  En **Stations Tasks** (Tareas de estaciones), haga clic en **Suspend all stations** (Suspender todas las estaciones).  
  
Después de que se haya suspendido una sesión de usuario, el usuario puede iniciar sesión en la misma estación o en otra y seguir trabajando en la sesión original.  
  
## <a name="see-also"></a>Vea también  
[Administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md)  
[Cierre la sesión o desconectar sesiones de usuario](Log-off-or-Disconnect-User-Sessions.md)