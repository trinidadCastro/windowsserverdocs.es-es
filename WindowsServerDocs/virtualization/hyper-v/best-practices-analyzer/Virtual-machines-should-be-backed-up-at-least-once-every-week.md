---
title: Se debe realizar una copia de seguridad de las máquinas virtuales al menos una vez cada semana
description: Obtenga información sobre qué hacer cuando no se ha realizado una copia de seguridad de una o más máquinas virtuales en la última semana.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
ms.date: 8/16/2016
ms.openlocfilehash: 9c2cc7836eca57b5d7ae7e180a8af5c6e3104996
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846306"
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

## <a name="issue"></a>Problema
*No se ha realizado una copia de seguridad de una o más máquinas virtuales en la última semana.*

## <a name="impact"></a>Impacto
*Podría producirse una pérdida de datos significativa si la máquina virtual encuentra un problema y no existe una copia de seguridad reciente. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Programe una copia de seguridad de las máquinas virtuales para que se ejecute al menos una vez a la semana. Puede omitir esta regla si esta máquina virtual es una réplica y se está realizando una copia de seguridad de su máquina virtual principal, o si se trata de la máquina virtual principal y se está realizando una copia de seguridad de su réplica.*



