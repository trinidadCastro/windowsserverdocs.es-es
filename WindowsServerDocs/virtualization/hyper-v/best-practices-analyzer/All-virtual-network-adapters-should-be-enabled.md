---
title: Todos los adaptadores de red virtuales deben estar habilitados
description: Obtenga información acerca de qué hacer cuando uno o varios adaptadores de red virtuales asociados a un adaptador de red físico están deshabilitados en el sistema operativo de administración.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
ms.date: 8/16/2016
ms.openlocfilehash: 6b3fcd786a1f7f294b7c35a4dc0a8b3cf0971c83
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834760"
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

## <a name="issue"></a>Problema

*Uno o más adaptadores de red virtuales asociados a un adaptador de red físico están deshabilitados en el sistema operativo de administración.*

## <a name="impact"></a>Impacto

*La configuración de este servidor no es óptima.*

El sistema operativo de administración no se puede conectar a una red física (externa) mediante uno de los adaptadores de red físicos del equipo porque está asociado a un adaptador de red virtual deshabilitado.

## <a name="resolution"></a>Solución

*Use la configuración de Internet de & de red para habilitar el adaptador de red virtual. O bien, use el administrador de conmutadores virtuales para volver a configurar el conmutador virtual externo de modo que no se comparta con el sistema operativo de administración.*



