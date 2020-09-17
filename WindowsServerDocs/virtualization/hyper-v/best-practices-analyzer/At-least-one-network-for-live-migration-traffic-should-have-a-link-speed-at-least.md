---
title: Al menos una red para el tráfico de migración en vivo debe tener una velocidad de vínculo de al menos 1 Gbps.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
ms.date: 8/16/2016
ms.openlocfilehash: cf1f09a635d6f6a46ee2a0bab2de53c898af9084
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746300"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Al menos una red para el tráfico de migración en vivo debe tener una velocidad de vínculo de al menos 1 Gbps.

>Se aplica a: Windows Server 2016



|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Ninguna de las redes para el tráfico de migración en vivo tiene una velocidad de vínculo de al menos 1 Gbps.*

## <a name="impact"></a>Impacto
*Las migraciones en vivo pueden producirse lentamente, lo que podría interrumpir la conexión de red debido a un tiempo de espera de conexión TCP.*

## <a name="resolution"></a>Solución
*Configure al menos una red de migración en vivo con una velocidad de 1 Gbps o más.*

Consulte la documentación del proveedor de hardware de red para averiguar si alguno de los adaptadores de red existentes puede admitir una velocidad de vínculo de al menos 1 Gbps.



