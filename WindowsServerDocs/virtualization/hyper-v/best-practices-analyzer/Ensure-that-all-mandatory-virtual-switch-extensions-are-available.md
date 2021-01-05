---
title: Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles
description: Obtenga información acerca de qué hacer cuando uno o más adaptadores de red virtuales están conectados a un conmutador virtual con extensiones obligatorias que están deshabilitadas o no instaladas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
ms.date: 8/16/2016
ms.openlocfilehash: 0af9d5823740ebcfaaf740f8738ea91d4d827baa
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846136"
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
*En primer lugar, asegúrese de que se ha instalado la extensión obligatoria en el host e instale la extensión si es necesario. A continuación, si la extensión obligatoria está deshabilitada, use el administrador de conmutadores virtuales o el cmdlet de Windows PowerShell Enable-VMSwitchExtension para habilitar la extensión.*



