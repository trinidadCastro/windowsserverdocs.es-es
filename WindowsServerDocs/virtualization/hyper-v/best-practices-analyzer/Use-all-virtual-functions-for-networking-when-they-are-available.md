---
title: Usar todas las funciones virtuales para redes cuando estén disponibles
description: Obtenga información acerca de qué hacer cuando no se están usando algunas funcionalidades de aceleración de hardware.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
ms.date: 8/16/2016
ms.openlocfilehash: 4252fd790c34bb9f4eca116eff84eb900033658b
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846308"
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

## <a name="issue"></a>Problema
*No se están usando algunas funciones de aceleración de hardware*

## <a name="impact"></a>Impacto
*Esta configuración puede hacer que el uso total de la CPU sea más alto de lo necesario. Es posible que el rendimiento de red no sea óptimo en las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Considere la posibilidad de configurar el adaptador de red virtual para SR-IOV si el hardware físico es compatible con SR-IOV y si esta configuración no entra en conflicto con las características de red que necesita la máquina virtual.*



