---
title: Al menos una red para el tráfico de migración en vivo debe tener una velocidad de vínculo de al menos 1 Gbps.
description: Obtenga información acerca de qué hacer cuando al menos una de las redes para el tráfico de migración en vivo tiene una velocidad de vínculo de al menos 1 Gbps.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
ms.date: 8/16/2016
ms.openlocfilehash: e18b9c259963c9a6ab1a3495f517e05a522be0c2
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834750"
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



