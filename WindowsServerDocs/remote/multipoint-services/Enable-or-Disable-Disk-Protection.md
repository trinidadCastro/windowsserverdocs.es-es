---
title: Habilitar o deshabilitar la protección de disco
description: Obtenga información sobre cómo usar la protección de disco con MultiPoint Services
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
ms.openlocfilehash: 78979c899d952781eb84a8da0746409a45309054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874436"
---
# <a name="enable-or-disable-disk-protection"></a>Habilitar o deshabilitar la protección de disco
La característica Protección de disco permite restablecer el sistema MultiPoint Services a un estado específico cada vez que se reinicia el sistema. Con Protección de disco, los usuarios pueden realizar cambios temporales en el sistema MultiPoint Services, que se descartan al reiniciarse el servidor. Algunos ejemplos de cambios que se descartarán al reiniciarse el sistema son: personalizar un perfil de usuario, guardar archivos, cambiar opciones de configuración o instalar aplicaciones.  
  
## <a name="enable-disk-protection"></a>Habilitar la protección de disco  
  
1.  En el Administrador de MultiPoint, haga clic en el **inicio** ficha y, a continuación, en * nombre de equipo ***tareas**, haga clic en **Habilitar protección de disco**.  
  
2.  Revise la información y, después, haga clic en **Siguiente**.  
  
Después de reiniciarse el sistema, los cambios realizados en el mismo, incluida la instalación de aplicaciones nuevas, se descartarán tras cada reinicio posterior.  
  
## <a name="disable-disk-protection"></a>Deshabilite la protección de disco  
  
1.  En el Administrador de MultiPoint, haga clic en el **inicio** ficha y, a continuación, en * nombre de equipo ***tareas**, haga clic en **deshabilitar protección de disco**.  
  
2.  Revise la información y, después, haga clic en **Siguiente**.  
  
Después de reiniciarse el sistema, los cambios realizados en el mismo, incluida la instalación de aplicaciones nuevas, serán permanentes y no se descartarán la próxima vez que se reinicie el sistema.  
  
