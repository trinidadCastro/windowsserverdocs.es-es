---
title: La configuración de PVLAN en un conmutador virtual debe ser coherente
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
ms.date: 8/16/2016
ms.openlocfilehash: 205917e793bfb0cc398f7309981109083189d439
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745590"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>La configuración de PVLAN en un conmutador virtual debe ser coherente

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*La red de área local virtual privada (PVLAN) no está configurada correctamente en uno o más adaptadores de red virtuales.*

## <a name="impact"></a>**Impacto**
*Es posible que PVLAN no Aísle correctamente el tráfico de red entre las máquinas virtuales.*

## <a name="resolution"></a>**Solución**
*Use el cmdlet de Windows PowerShell, Set-VMNetworkAdapterVlan, para configurar PVLAN correctamente.*



