---
title: No se deben configurar puertos serie en máquinas virtuales de generación 2
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d9cfb8db2dc3fdff32544670d9443632c2d2264e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861788"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>No se deben configurar puertos serie en máquinas virtuales de generación 2

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Una o varias máquinas virtuales de generación 2 tienen un puerto serie configurado.*  
  
## <a name="impact"></a>**Impacto**  
*El rendimiento puede verse afectado en las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Si esto es intencionado, no es necesario realizar ninguna otra acción. En caso contrario, considere la posibilidad de usar el administrador de Hyper-V o Windows PowerShell para quitar la cadena de conexión de los puertos serie de la máquina virtual.*  
  


