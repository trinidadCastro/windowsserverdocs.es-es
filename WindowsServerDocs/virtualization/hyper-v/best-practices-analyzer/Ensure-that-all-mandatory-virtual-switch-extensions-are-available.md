---
title: Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 48c8ee80a0044c067d29730c1ec79bd67fdc062b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950259"
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

## <a name="issue"></a>Incidencia
*Uno o más adaptadores de red virtual están conectados a un conmutador virtual con extensiones obligatorias que están deshabilitadas o no instaladas.*

## <a name="impact"></a>Impacto
*El tráfico de red está bloqueado en uno o varios adaptadores de red virtual de las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*En primer lugar, asegúrese de que se ha instalado la extensión obligatoria en el host e instale la extensión si es necesario. A continuación, si la extensión obligatoria está deshabilitada, use el administrador de conmutadores virtuales o el cmdlet enable-VMSwitchExtension de Windows PowerShell para habilitar la extensión.*



