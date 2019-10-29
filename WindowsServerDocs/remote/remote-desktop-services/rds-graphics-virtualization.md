---
title: 'Servicios de Escritorio remoto: aceleración de GPU'
description: Información que le ayuda a elegir la opción de virtualización de gráficos adecuada para la implementación de RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/21/2019
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: 914994c66be0a56856ec68d08be9dc5465df015e
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923847"
---
# <a name="remote-desktop-services---gpu-acceleration"></a>Servicios de Escritorio remoto: aceleración de GPU

Los Servicios de Escritorio remoto funcionan con la aceleración de gráficos nativa, así como con las tecnologías de virtualización de gráficos compatibles con Windows Server. Para obtener información sobre esas tecnologías, sus diferencias y cómo implementarlas, consulta [Plan de aceleración de GPU en Windows Server](../../virtualization/hyper-v/plan/plan-for-gpu-acceleration-in-windows-server.md).

Al planear la aceleración de gráficos en el entorno de RDS, la elección de la escala de usuario y las cargas de trabajo de usuario controlarán la elección de tecnología de representación de gráficos:

![Consideraciones de representación de gráficos (compara la escala del usuario y los requisitos de GPU para determinar la mejor tecnología de GPU para el entorno)](media/rds-gpu.png)
