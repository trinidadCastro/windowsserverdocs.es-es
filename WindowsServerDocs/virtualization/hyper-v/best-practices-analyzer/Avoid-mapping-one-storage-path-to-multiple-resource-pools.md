---
title: Evite asignar una ruta de acceso de almacenamiento a varios grupos de recursos
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e89c0382d20d586d8c0b50396ddbd56d6fdadf0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365256"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite asignar una ruta de acceso de almacenamiento a varios grupos de recursos

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.
  
## <a name="issue"></a>**Problema**  
*Una ruta de acceso al archivo de almacenamiento se asigna a varios grupos de recursos.*  
  
## <a name="impact"></a>**Impacto**  
*Para el tipo de grupo de almacenamiento especificado, los siguientes grupos primarios y secundarios comparten la misma ruta de acceso de almacenamiento:*  
  
\<list de los grupos >  
  
## <a name="resolution"></a>**Resolución**  
*Use Windows PowerShell para volver a configurar los grupos de recursos de almacenamiento de modo que varios grupos no usen la misma ruta de acceso de almacenamiento.*  
  


