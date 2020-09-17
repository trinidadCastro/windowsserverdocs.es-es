---
title: No se deben configurar puertos serie en máquinas virtuales de generación 2
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
ms.date: 8/16/2016
ms.openlocfilehash: 2dbb9aae488922daeff74242d74d38685a371497
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90744110"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>No se deben configurar puertos serie en máquinas virtuales de generación 2

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
*Una o varias máquinas virtuales de generación 2 tienen un puerto serie configurado.*

## <a name="impact"></a>**Impacto**
*El rendimiento puede verse afectado en las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Solución**
*Si esto es intencionado, no es necesario realizar ninguna otra acción. En caso contrario, considere la posibilidad de usar el administrador de Hyper-V o Windows PowerShell para quitar la cadena de conexión de los puertos serie de la máquina virtual.*



