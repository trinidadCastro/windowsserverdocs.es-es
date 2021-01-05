---
title: Habilitar todos los adaptadores de red virtuales configurados para una máquina virtual
description: Obtenga información acerca de qué hacer cuando uno o varios adaptadores de red se pueden deshabilitar en una máquina virtual.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
ms.date: 8/16/2016
ms.openlocfilehash: 300aad537c5e111003db60b1ba947dbea447ada9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846371"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Habilitar todos los adaptadores de red virtuales configurados para una máquina virtual

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Es posible que uno o más adaptadores de red estén deshabilitados en una máquina virtual.*

## <a name="impact"></a>Impacto

*Es posible que las siguientes máquinas virtuales no tengan conectividad de red:*

\<list of virtual machine names>

## <a name="resolution"></a>Solución

*Use Device Manager del sistema operativo invitado para habilitar todos los adaptadores de red virtuales. Si el adaptador no es necesario, use el administrador de Hyper-V para quitarlo de la máquina virtual.*



