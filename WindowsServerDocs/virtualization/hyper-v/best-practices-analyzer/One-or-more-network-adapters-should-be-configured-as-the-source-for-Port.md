---
title: Uno o más adaptadores de red deben configurarse como origen para la creación de reflejo del puerto
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
ms.date: 8/16/2016
ms.openlocfilehash: c8a04001986ab4d3722c9e51a5d5ceb7cbd0bf02
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746250"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Uno o más adaptadores de red deben configurarse como origen para la creación de reflejo del puerto

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
*Una o varias máquinas virtuales tienen un adaptador de red configurado como destino para la creación de reflejo del puerto, pero no hay ningún origen correspondiente en el conmutador virtual.*

## <a name="impact"></a>**Impacto**
*La creación de reflejo del puerto no funcionará correctamente para los siguientes conmutadores virtuales y máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Solución**
*Use Windows PowerShell o el administrador de Hyper-V para completar o corregir la configuración de creación de reflejo del puerto.*



