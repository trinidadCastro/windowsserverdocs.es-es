---
title: Asegurarse de que el controlador de función virtual funciona correctamente cuando una máquina virtual está configurada para usar SR-IOV
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 8fc14e14ff66b59099ebad1f3b9643b94427346a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954492"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Asegurarse de que el controlador de función virtual funciona correctamente cuando una máquina virtual está configurada para usar SR-IOV

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
*El controlador de función virtual no funciona correctamente en el sistema operativo invitado de una o varias máquinas virtuales.*

## <a name="impact"></a>Impacto
*El rendimiento de red no es óptimo en las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*En el sistema operativo invitado, realice lo siguiente: Compruebe que estén instalados los controladores adecuados y que todos los dispositivos de red estén habilitados, y compruebe si hay errores o advertencias en el registro de eventos.*



