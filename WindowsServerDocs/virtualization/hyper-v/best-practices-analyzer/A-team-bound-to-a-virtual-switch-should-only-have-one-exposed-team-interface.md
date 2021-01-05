---
title: Un equipo enlazado a un conmutador virtual solo debe tener una interfaz de equipo expuesta
description: Obtenga información acerca de qué hacer cuando uno o varios conmutadores virtuales están enlazados a un equipo que tiene varias interfaces de equipo.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
ms.date: 8/16/2016
ms.openlocfilehash: af7b7b80559004c765a4a964b0e72827d74136e7
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834770"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Un equipo enlazado a un conmutador virtual solo debe tener una interfaz de equipo expuesta

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
*Uno o varios conmutadores virtuales están enlazados a un equipo que tiene varias interfaces de equipo.*

## <a name="impact"></a>Impacto
*Es posible que los siguientes conmutadores virtuales no tengan acceso a las VLAN y al ancho de banda usado por otras interfaces de equipo:*

\<list of virtual switches>

## <a name="resolution"></a>Solución
*Use el cmdlet de Windows PowerShell Remove-NetLbfoTeamNic para quitar todas las interfaces de equipo del equipo que no sean la interfaz de equipo predeterminada.*



