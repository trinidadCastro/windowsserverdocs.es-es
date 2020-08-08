---
title: Un equipo enlazado a un conmutador virtual solo debe tener una interfaz de equipo expuesta
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7ec4c25a86bf90f1b2416e0d53ded8f5319960ad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87968522"
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

## <a name="issue"></a>Incidencia
*Uno o varios conmutadores virtuales están enlazados a un equipo que tiene varias interfaces de equipo.*

## <a name="impact"></a>Impacto
*Es posible que los siguientes conmutadores virtuales no tengan acceso a las VLAN y al ancho de banda usado por otras interfaces de equipo:*

\<list of virtual switches>

## <a name="resolution"></a>Resolución
*Use el cmdlet de Windows PowerShell Remove-NetLbfoTeamNic para quitar todas las interfaces de equipo del equipo que no sean la interfaz de equipo predeterminada.*



