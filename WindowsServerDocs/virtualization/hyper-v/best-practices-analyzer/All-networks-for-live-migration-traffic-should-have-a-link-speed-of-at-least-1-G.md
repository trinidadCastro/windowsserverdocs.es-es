---
title: Todas las redes para el tráfico de migración en vivo deben tener una velocidad de vínculo de al menos 1 Gbps
description: Obtenga información acerca de qué hacer cuando ninguna de las redes para el tráfico de migración en vivo tiene una velocidad de vínculo de al menos 1 Gbps.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
ms.date: 8/16/2016
ms.openlocfilehash: 9c5b581635920e4030da8792e37feb8d09170c51
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834910"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Todas las redes para el tráfico de migración en vivo deben tener una velocidad de vínculo de al menos 1 Gbps

> Se aplica a: Windows Server 2016

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Ninguna de las redes para el tráfico de migración en vivo tiene una velocidad de vínculo de al menos 1 Gbps.*

## <a name="impact"></a>Impacto
*Las migraciones en vivo pueden producirse lentamente, lo que podría interrumpir la conexión de red debido a un tiempo de espera de conexión TCP.*

## <a name="resolution"></a>Solución
*Configure al menos una red de migración en vivo con una velocidad de 1 Gbps o más.*

Consulte la documentación del proveedor de hardware de red para averiguar si alguno de los adaptadores de red existentes puede admitir una velocidad de vínculo de al menos 1 Gbps.



