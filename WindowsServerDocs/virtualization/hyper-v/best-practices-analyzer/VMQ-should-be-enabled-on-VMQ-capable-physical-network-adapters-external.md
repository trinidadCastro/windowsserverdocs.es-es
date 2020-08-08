---
title: VMQ debe estar habilitado en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ae8c5005f9038ac2b82fd3f56016da357a4a9364
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960238"
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

## <a name="resolution"></a>**Resolución**
*Habilite VMQ con el cmdlet enable-NetAdapterVmq de Windows PowerShell o con la interfaz de usuario de propiedades avanzadas del adaptador de red.*



