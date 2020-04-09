---
title: Habilitar o deshabilitar la protección de disco
description: Aprenda a usar la protección de disco con Multipoint Services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 00aba4c4-0244-4b39-8c85-c46fd96e1d6a
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 7f51491ff81077ec3d6eeb5d42322350a68bb018
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859258"
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
  
