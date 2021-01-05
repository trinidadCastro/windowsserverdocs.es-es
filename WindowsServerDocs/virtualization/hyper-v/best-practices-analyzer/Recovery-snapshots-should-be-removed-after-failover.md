---
title: Las instantáneas de recuperación se deben quitar después de la conmutación por error
description: Obtenga información acerca de qué hacer cuando una máquina virtual conmutada por error tiene una o varias instantáneas de recuperación.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
ms.date: 8/16/2016
ms.openlocfilehash: e37e59b8796913c2d46914834f4467e927f7e32b
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846276"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Las instantáneas de recuperación se deben quitar después de la conmutación por error

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operations|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Una máquina virtual conmutada por error tiene una o varias instantáneas de recuperación.*

## <a name="impact"></a>**Impacto**
*Se puede agotar el espacio disponible en el disco físico que almacena los archivos de instantáneas. Si esto ocurre, no se pueden realizar más operaciones de disco en el almacenamiento físico. Cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Para cada máquina virtual conmutada por error, use el cmdlet Complete-VMFailover en Windows PowerShell para quitar las instantáneas de recuperación e indicar la finalización de la conmutación por error.*



