---
title: VMQ debe estar habilitado en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
ms.date: 8/16/2016
ms.openlocfilehash: 169f2ea06bf35c7bbc9bcaec354ca66e118c75d2
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746770"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>VMQ debe estar habilitado en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Los siguientes adaptadores de red son capaces de Virtual Machine Queue (VMQ) pero la funcionalidad está deshabilitada.*

## <a name="impact"></a>**Impacto**
*Windows no puede sacar el máximo partido de las descargas de hardware disponibles en los siguientes adaptadores de red:*

\<list of network adapters>

## <a name="resolution"></a>**Solución**
*Habilite VMQ con el cmdlet enable-NetAdapterVmq de Windows PowerShell o con la interfaz de usuario de propiedades avanzadas del adaptador de red.*



