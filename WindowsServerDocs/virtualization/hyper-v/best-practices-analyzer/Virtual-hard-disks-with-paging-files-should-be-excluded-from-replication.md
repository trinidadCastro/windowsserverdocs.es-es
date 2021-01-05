---
title: Los discos duros virtuales con archivos de paginación se deben excluir de la replicación
description: Obtenga información sobre qué hacer cuando los archivos de paginación experimenten un gran volumen de actividad de entrada/salida, lo que requerirá innecesariamente recursos mucho mayores para participar en la replicación.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
ms.date: 8/16/2016
ms.openlocfilehash: c710891396ffe8914819796c18abaf0ddc41fea4
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845748"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Los discos duros virtuales con archivos de paginación se deben excluir de la replicación

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Información|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Los archivos de paginación deben excluirse de la replicación, pero no se han excluido los discos.*

## <a name="impact"></a>Impacto
*Los archivos de paginación experimentan un gran volumen de actividad de entrada/salida, lo que requerirá innecesariamente recursos mucho mayores para participar en la replicación. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Si todavía no lo ha hecho, cree un disco duro virtual independiente para el archivo de paginación de Windows. Si ya se ha completado la replicación inicial, use el administrador de Hyper-V para quitar la replicación. A continuación, vuelva a configurar la replicación y excluya el disco duro virtual con el archivo de paginación de la replicación.*



