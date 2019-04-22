---
title: Evitar la asignación de una ruta de acceso de almacenamiento a varios grupos de recursos
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7c012836309f722e55c28b2ddbe3d54de641b4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823966"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evitar la asignación de una ruta de acceso de almacenamiento a varios grupos de recursos

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>**Problema**  
*Una ruta de acceso del archivo de almacenamiento se asigna a varios grupos de recursos.*  
  
## <a name="impact"></a>**Impact**  
*Para el tipo de grupo de almacenamiento especificada, los siguientes grupos primarios y secundarios comparten la misma ruta de acceso de almacenamiento:*  
  
\<lista de grupos de >  
  
## <a name="resolution"></a>**Resolución**  
*Usar Windows PowerShell para volver a configurar los grupos de recursos de almacenamiento para que varios grupos no use la misma ruta de acceso de almacenamiento.*  
  


