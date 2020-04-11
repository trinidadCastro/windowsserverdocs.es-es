---
title: 'Servicios de Escritorio remoto: aceleración de GPU'
description: Información que le ayuda a elegir la opción de virtualización de gráficos adecuada para la implementación de RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/21/2019
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: 5403be897df48972edd4c5b37744a83d8ad25972
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861388"
---
# <a name="remote-desktop-services---gpu-acceleration"></a>Servicios de Escritorio remoto: aceleración de GPU

Los Servicios de Escritorio remoto funcionan con la aceleración de gráficos nativa, así como con las tecnologías de virtualización de gráficos compatibles con Windows Server. Para obtener información sobre esas tecnologías, sus diferencias y cómo implementarlas, consulta [Plan de aceleración de GPU en Windows Server](../../virtualization/hyper-v/plan/plan-for-gpu-acceleration-in-windows-server.md).

Al planear la aceleración de gráficos en el entorno de RDS, la elección de la escala de usuario y las cargas de trabajo de usuario controlarán la elección de tecnología de representación de gráficos:

![Consideraciones de representación de gráficos (compara la escala del usuario y los requisitos de GPU para determinar la mejor tecnología de GPU para el entorno)](media/rds-gpu.png)
