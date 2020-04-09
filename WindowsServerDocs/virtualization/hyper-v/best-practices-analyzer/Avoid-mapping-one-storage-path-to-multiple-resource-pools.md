---
title: Evite asignar una ruta de acceso de almacenamiento a varios grupos de recursos
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53ef04a2dde875b26dd109075a2cfa4484ebd5cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857758"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite asignar una ruta de acceso de almacenamiento a varios grupos de recursos

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.
  
## <a name="issue"></a>**Problema**  
*Una ruta de acceso al archivo de almacenamiento se asigna a varios grupos de recursos.*  
  
## <a name="impact"></a>**Impacto**  
*Para el tipo de grupo de almacenamiento especificado, los siguientes grupos primarios y secundarios comparten la misma ruta de acceso de almacenamiento:*  
  
\<lista de grupos >  
  
## <a name="resolution"></a>**Resolución**  
*Use Windows PowerShell para volver a configurar los grupos de recursos de almacenamiento de modo que varios grupos no usen la misma ruta de acceso de almacenamiento.*  
  


