---
title: El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales
description: Obtenga información acerca de qué hacer cuando no hay suficientes funciones virtuales disponibles para el número de máquinas virtuales en ejecución configuradas para la virtualización de e/s de raíz única (SR-IOV).
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
ms.date: 8/16/2016
ms.openlocfilehash: 783b5dbf170756e922b448d73881543b5a811ce9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845891"
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



