---
title: Habilitar o deshabilitar la protección de disco
description: Aprenda a usar la protección de disco con Multipoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00aba4c4-0244-4b39-8c85-c46fd96e1d6a
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d037b0843f5ba50c98e0d6e7cb10836c8d6fa23a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871751"
---
# <a name="enable-or-disable-disk-protection"></a>Habilitar o deshabilitar la protección de disco
La característica Protección de disco permite restablecer el sistema MultiPoint Services a un estado específico cada vez que se reinicia el sistema. Con Protección de disco, los usuarios pueden realizar cambios temporales en el sistema MultiPoint Services, que se descartan al reiniciarse el servidor. Entre los ejemplos de cambios que se descartarán cuando se reinicie el servidor se incluyen personalizar el perfil de un usuario, guardar archivos, cambiar la configuración o instalar aplicaciones.  
  
## <a name="enable-disk-protection"></a>Habilitar protección de disco  
  
1.  En Multipoint Manager, haga clic en la pestaña **Inicio** y, a continuación, en * nombre del equipo ***tareas**, haga clic en **Habilitar protección de disco**.  
  
2.  Revise la información y, después, haga clic en **Siguiente**.  
  
Después de reiniciarse el sistema, los cambios realizados en el mismo, incluida la instalación de aplicaciones nuevas, se descartarán tras cada reinicio posterior.  
  
## <a name="disable-disk-protection"></a>Deshabilitar la protección de disco  
  
1.  En Multipoint Manager, haga clic en la pestaña **Inicio** y, a continuación, en * nombre del equipo ***tareas**, haga clic en **deshabilitar protección de disco**.  
  
2.  Revise la información y, después, haga clic en **Siguiente**.  
  
Después de reiniciarse el sistema, los cambios realizados en el mismo, incluida la instalación de aplicaciones nuevas, serán permanentes y no se descartarán la próxima vez que se reinicie el sistema.  
  
