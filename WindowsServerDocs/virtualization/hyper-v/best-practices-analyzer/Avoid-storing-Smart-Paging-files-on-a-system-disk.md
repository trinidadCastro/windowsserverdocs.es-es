---
title: Evitar el almacenamiento de archivos de paginación inteligente en un disco del sistema
description: Obtenga información acerca de qué hacer cuando la configuración de memoria de una o varias máquinas virtuales puede requerir el uso de la paginación inteligente si se reinicia la máquina virtual y la ubicación especificada para el archivo de paginación inteligente es el disco del sistema del servidor que ejecuta Hyper-V.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
ms.date: 8/16/2016
ms.openlocfilehash: 990067509f0fbfe9ba19781da23ea3ea0b6e4091
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834600"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evitar el almacenamiento de archivos de paginación inteligente en un disco del sistema

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operations|

En las secciones siguientes, cursiva indica el texto que aparece en la herramienta Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*La configuración de memoria de una o varias máquinas virtuales podría requerir el uso de la paginación inteligente si se reinicia la máquina virtual y la ubicación especificada para el archivo de paginación inteligente es el disco del sistema del servidor que ejecuta Hyper-V.*

## <a name="impact"></a>Impacto
*El uso del disco del sistema para la paginación inteligente podría provocar problemas en el servidor que ejecuta Hyper-V. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Vuelva a configurar las máquinas virtuales para almacenar los archivos de paginación inteligente en un disco que no sea del sistema.*



