---
title: La interfaz de equipo enlazada a un conmutador virtual debe estar en el modo predeterminado
description: Obtenga información acerca de qué hacer cuando algunos conmutadores virtuales están enlazados a una interfaz de equipo, pero la interfaz de equipo no pasa el tráfico de todas las VLAN a los conmutadores virtuales.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
ms.date: 8/16/2016
ms.openlocfilehash: e12ed7286a3aba4d6bb4f4bd3c72481326f44e0f
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845900"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>La interfaz de equipo enlazada a un conmutador virtual debe estar en el modo predeterminado

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
*Algunos conmutadores virtuales están enlazados a una interfaz de equipo, pero la interfaz de equipo no pasa el tráfico de todas las VLAN a los conmutadores virtuales.*

## <a name="impact"></a>**Impacto**
*Los siguientes conmutadores virtuales no pueden tener acceso a todas las VLAN: \n{0}*

## <a name="resolution"></a>**Resolución**
*Use Administrador del servidor o el cmdlet de Windows PowerShell Set-NetLbfoTeamNic para restablecer la interfaz de equipo en el modo predeterminado.*



