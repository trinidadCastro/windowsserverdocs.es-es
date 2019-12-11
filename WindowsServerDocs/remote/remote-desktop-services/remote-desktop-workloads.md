---
title: Cargas de trabajo de Escritorio remoto
description: Breve descripción de los distintos tipos de cargas de trabajo de las máquinas virtuales que administra Escritorio remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.tgt_pltfrm: na
ms.topic: article
author: Heidilohr
manager: daveba
ms.openlocfilehash: 32781946ad2c49ec51e11dfe1e8af078425d35b0
ms.sourcegitcommit: cbf0c7c37797c22af989639fac82fc0eee94497f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74700934"
---
# <a name="remote-desktop-workloads"></a>Cargas de trabajo de Escritorio remoto

Los usuarios pueden ejecutar diferentes tipos de cargas de trabajo en las máquinas virtuales administradas por Servicios de Escritorio remoto o Windows Virtual Desktop. Escala la implementación según la necesidad esperada de cada tipo de usuario. En la tabla siguiente se proporcionan ejemplos de una serie de tipos de carga de trabajo que te ayudarán a calcular el tamaño que deben tener las máquinas virtuales. Después de configurar las máquinas virtuales, debes supervisar continuamente su uso real y ajustar su tamaño en consecuencia. Si acabas necesitando una máquina virtual más grande o más pequeña, puedes ampliar o reducir la implementación existente en Azure.

En la tabla siguiente se describe cada carga de trabajo. Los "usuarios de ejemplo" son los tipos de usuarios que pueden considerar cada carga de trabajo más útil. Las "aplicaciones de ejemplo" son los tipos de aplicaciones que funcionan mejor para cada carga de trabajo.

| Tipo de carga de trabajo | Usuarios de ejemplo | Aplicaciones de ejemplo |
| --- | --- | --- |
| Claro | Usuarios que realizan tareas básicas de entrada de datos | Aplicaciones de entrada de base de datos, interfaces de línea de comandos |
| Medio | Consultores e investigadores de mercados | Aplicaciones de entrada de base de datos, interfaces de línea de comandos, Microsoft Word, páginas web estáticas |
| Pesado | Ingenieros de software, creadores de contenido | Aplicaciones de entrada de base de datos, interfaces de línea de comandos, Microsoft Word, páginas web estáticas, Microsoft Outlook, Microsoft PowerPoint, páginas web dinámicas |
| Alimentación | Diseñadores gráficos, responsables de modelos 3D, investigadores de aprendizaje automático | Aplicaciones de entrada de base de datos, interfaces de línea de comandos, Microsoft Word, páginas web estáticas, Microsoft Outlook, Microsoft PowerPoint, páginas web dinámicas, Adobe Photoshop, Adobe Illustrator, diseño asistido por ordenador (CAD), fabricación asistida por ordenador (CAM) |

Para obtener información acerca de las recomendaciones de ajuste de tamaño, consulta [guía de tamaños de máquina virtual](virtual-machine-recs.md).
