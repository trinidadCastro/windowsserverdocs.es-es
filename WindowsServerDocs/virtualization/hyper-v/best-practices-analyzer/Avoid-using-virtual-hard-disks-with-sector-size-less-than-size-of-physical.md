---
title: Evite el uso de discos duros virtuales con un tamaño de sector menor que el tamaño de sector del almacenamiento físico que almacena el archivo de disco duro virtual.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
ms.date: 8/16/2016
ms.openlocfilehash: 946168aa200ec80e5d2c9a69e0ecfad2477dcb6e
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745890"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evite el uso de discos duros virtuales con un tamaño de sector menor que el tamaño de sector del almacenamiento físico que almacena el archivo de disco duro virtual.

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Operating** <br />**Sistema**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Uno o más discos duros virtuales tienen un tamaño de sector físico menor que el tamaño de sector físico del almacenamiento en el que se encuentra el archivo de disco duro virtual.*

## <a name="impact"></a>**Impacto**
*Pueden producirse problemas de rendimiento en la máquina virtual o la aplicación que usa el disco duro virtual. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Solución**
*Realice una de las siguientes acciones:*

-   *Realizar una migración de almacenamiento para trasladar el disco duro virtual a un nuevo sistema físico*

-   *Usar Windows PowerShell o WMI para habilitar un disco duro virtual con formato VHDX para informar de un tamaño de sector específico*

-   *Usar una configuración del registro para permitir que un disco duro virtual con formato VHD informe de un tamaño de sector físico de 4k*



