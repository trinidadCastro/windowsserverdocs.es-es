---
title: Usar todas las funciones virtuales para redes cuando estén disponibles
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6be1d32b22d5619ff40d0121bcc60a6bfa3c225f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960308"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Usar todas las funciones virtuales para redes cuando estén disponibles

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
*No se están usando algunas funciones de aceleración de hardware*

## <a name="impact"></a>Impacto
*Esta configuración puede hacer que el uso total de la CPU sea más alto de lo necesario. Es posible que el rendimiento de red no sea óptimo en las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Considere la posibilidad de configurar el adaptador de red virtual para SR-IOV si el hardware físico es compatible con SR-IOV y si esta configuración no entra en conflicto con las características de red que necesita la máquina virtual.*



