---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: d1bfc74c4daa751e3081b26c87dd0d2c88f5f095
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879856"
---
Las opciones de adaptador del modo de espera son **None (todos los adaptadores activos)** o la selección de un adaptador de red específico en el equipo NIC que actúa como un adaptador de modo de espera. Cuando se configura una NIC como un adaptador de modo de espera, todos los otros miembros del equipo no seleccionados están activos y se envía ningún tráfico de red o se procesa el adaptador hasta que se produce un error en un adaptador de red activo. Una vez que se produce un error en un adaptador de red activo, la NIC en espera se activa y los procesos de tráfico de red. Cuando se restauran todos los miembros del equipo al servicio, el miembro del equipo en espera se devuelve al código de estado en espera.  