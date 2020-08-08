---
title: Se debe realizar una copia de seguridad de las máquinas virtuales al menos una vez cada semana
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fccc0bb7c46206df210f699cdda84507343c342a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960208"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Se debe realizar una copia de seguridad de las máquinas virtuales al menos una vez cada semana

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia
*No se ha realizado una copia de seguridad de una o más máquinas virtuales en la última semana.*

## <a name="impact"></a>Impacto
*Podría producirse una pérdida de datos significativa si la máquina virtual encuentra un problema y no existe una copia de seguridad reciente. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Programe una copia de seguridad de las máquinas virtuales para que se ejecute al menos una vez a la semana. Puede omitir esta regla si esta máquina virtual es una réplica y se está realizando una copia de seguridad de su máquina virtual principal, o si se trata de la máquina virtual principal y se está realizando una copia de seguridad de su réplica.*



