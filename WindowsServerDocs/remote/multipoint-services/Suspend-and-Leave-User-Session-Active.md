---
title: Suspender y dejar activa la sesión de usuario
description: Obtenga información sobre cómo suspender a un usuario de una sesión de Multipoint sin desconectarlo.
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2d2771fd60f4d8c11c602a4c5d55f3b5ae2d8b11
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861628"
---
# <a name="suspend-and-leave-user-session-active"></a>Suspender y dejar activa la sesión de usuario
Puede desconectar o suspender a los usuarios del sistema Multipoint Services si no desea finalizar las sesiones de los usuarios. Un usuario también puede desconectar la sesión, en lugar de ser el usuario administrativo el que la desconecte. Mientras se suspende una sesión de usuario, la sesión permanece activa en la memoria del equipo del sistema Multipoint Services hasta que el equipo se apaga o se reinicia. En ese momento, todas las sesiones suspendidas finalizan y se perderá todo el trabajo que no se haya guardado.  
  
1.  Abra Multipoint Manager en modo de estación y, después, haga clic en la pestaña **estaciones** .  
  
2.  En la columna **Equipo**, haga clic en el nombre del equipo cuyas sesiones quiere suspender.  
  
3.  En **Stations Tasks** (Tareas de estaciones), haga clic en **Suspend all stations** (Suspender todas las estaciones).  
  
Después de que se haya suspendido una sesión de usuario, el usuario puede iniciar sesión en la misma estación o en otra y seguir trabajando en la sesión original.  
  
## <a name="see-also"></a>Consulta también  
[Administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md)  
[Cerrar sesión o desconectar sesiones de usuario](Log-off-or-Disconnect-User-Sessions.md)