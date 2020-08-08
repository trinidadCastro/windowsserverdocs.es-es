---
title: Evitar el almacenamiento de archivos de paginación inteligente en un disco del sistema
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 5ac7f8c9f6fb2edf2f6550cede9adfdc5be321f6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946078"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evitar el almacenamiento de archivos de paginación inteligente en un disco del sistema

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operaciones|

En las secciones siguientes, cursiva indica el texto que aparece en la herramienta Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia
*La configuración de memoria de una o varias máquinas virtuales podría requerir el uso de la paginación inteligente si se reinicia la máquina virtual y la ubicación especificada para el archivo de paginación inteligente es el disco del sistema del servidor que ejecuta Hyper-V.*

## <a name="impact"></a>Impacto
*El uso del disco del sistema para la paginación inteligente podría provocar problemas en el servidor que ejecuta Hyper-V. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Vuelva a configurar las máquinas virtuales para almacenar los archivos de paginación inteligente en un disco que no sea del sistema.*



