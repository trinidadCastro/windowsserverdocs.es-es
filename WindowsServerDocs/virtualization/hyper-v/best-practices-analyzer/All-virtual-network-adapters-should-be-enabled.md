---
title: Todos los adaptadores de red virtuales deben estar habilitados
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 286a0dc099eb6350fe7f5adef925a2d38d99a3ca
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946101"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Todos los adaptadores de red virtuales deben estar habilitados

>Se aplica a: Windows Server 2016



|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia

*Uno o más adaptadores de red virtuales asociados a un adaptador de red físico están deshabilitados en el sistema operativo de administración.*

## <a name="impact"></a>Impacto

*La configuración de este servidor no es óptima.*

El sistema operativo de administración no se puede conectar a una red física (externa) mediante uno de los adaptadores de red físicos del equipo porque está asociado a un adaptador de red virtual deshabilitado.

## <a name="resolution"></a>Resolución

*Use la configuración de Internet de & de red para habilitar el adaptador de red virtual. O bien, use el administrador de conmutadores virtuales para volver a configurar el conmutador virtual externo de modo que no se comparta con el sistema operativo de administración.*



