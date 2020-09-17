---
title: Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
ms.date: 8/16/2016
ms.openlocfilehash: 8eeadf6ed8b90ef217d694e2f14b38314b8be7a5
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746860"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles

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
*Uno o más adaptadores de red virtual están conectados a un conmutador virtual con extensiones obligatorias que están deshabilitadas o no instaladas.*

## <a name="impact"></a>Impacto
*El tráfico de red está bloqueado en uno o varios adaptadores de red virtual de las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*En primer lugar, asegúrese de que se ha instalado la extensión obligatoria en el host e instale la extensión si es necesario. A continuación, si la extensión obligatoria está deshabilitada, use el administrador de conmutadores virtuales o el cmdlet enable-VMSwitchExtension de Windows PowerShell para habilitar la extensión.*



