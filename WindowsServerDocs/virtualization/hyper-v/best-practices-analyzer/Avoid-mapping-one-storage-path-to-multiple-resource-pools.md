---
title: Evite asignar una ruta de acceso de almacenamiento a varios grupos de recursos
description: Obtenga información acerca de qué hacer cuando una ruta de acceso a un archivo de almacenamiento se asigna a varios grupos de recursos.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
ms.date: 8/16/2016
ms.openlocfilehash: f39ea4da5a7b966a29b0ad943d40b02556c9460d
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834640"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite asignar una ruta de acceso de almacenamiento a varios grupos de recursos

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
*Una ruta de acceso al archivo de almacenamiento se asigna a varios grupos de recursos.*

## <a name="impact"></a>**Impacto**
*Para el tipo de grupo de almacenamiento especificado, los siguientes grupos primarios y secundarios comparten la misma ruta de acceso de almacenamiento:*

\<list of pools>

## <a name="resolution"></a>**Resolución**
*Use Windows PowerShell para volver a configurar los grupos de recursos de almacenamiento de modo que varios grupos no usen la misma ruta de acceso de almacenamiento.*



