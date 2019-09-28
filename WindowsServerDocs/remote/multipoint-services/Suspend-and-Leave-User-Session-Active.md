---
title: Suspender y dejar activa la sesión de usuario
description: Obtenga información sobre cómo suspender a un usuario de una sesión de Multipoint sin desconectarlo.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0ef9d98584df568438cc3c905a7c86cd58f53343
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394931"
---
# <a name="suspend-and-leave-user-session-active"></a>Suspender y dejar activa la sesión de usuario
Puede desconectar o suspender a los usuarios del sistema Multipoint Services si no desea finalizar las sesiones de los usuarios. Un usuario también puede desconectar la sesión, en lugar de ser el usuario administrativo el que la desconecte. Mientras se suspende una sesión de usuario, la sesión permanece activa en la memoria del equipo del sistema Multipoint Services hasta que el equipo se apaga o se reinicia. En ese momento, todas las sesiones suspendidas finalizan y se perderá todo el trabajo que no se haya guardado.  
  
1.  Abra Multipoint Manager en modo de estación y, después, haga clic en la pestaña **estaciones** .  
  
2.  En la columna **Equipo**, haga clic en el nombre del equipo cuyas sesiones quiere suspender.  
  
3.  En **Stations Tasks** (Tareas de estaciones), haga clic en **Suspend all stations** (Suspender todas las estaciones).  
  
Después de que se haya suspendido una sesión de usuario, el usuario puede iniciar sesión en la misma estación o en otra y seguir trabajando en la sesión original.  
  
## <a name="see-also"></a>Vea también  
[Administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md)  
[Cerrar sesión o desconectar sesiones de usuario](Log-off-or-Disconnect-User-Sessions.md)