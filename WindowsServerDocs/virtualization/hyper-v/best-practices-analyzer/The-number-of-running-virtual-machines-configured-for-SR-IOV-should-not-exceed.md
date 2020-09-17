---
title: El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
ms.date: 8/16/2016
ms.openlocfilehash: 432bf4132e8c19a326fda646960b30315d3952b1
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745810"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*No hay suficientes funciones virtuales disponibles para el número de máquinas virtuales en ejecución configuradas para la virtualización de e/s de raíz única (SR-IOV).*

## <a name="impact"></a>Impacto
*Es posible que el rendimiento de red no sea óptimo en las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Considere la posibilidad de deshabilitar SR-IOV en una o varias máquinas virtuales que no requieran una función virtual de SR-IOV.*



