---
title: Los discos duros virtuales con archivos de paginación se deben excluir de la replicación
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 94e03cf9de3991d003fad9019b9af33fad2f6bae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855028"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Los discos duros virtuales con archivos de paginación se deben excluir de la replicación

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Información|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Los archivos de paginación deben excluirse de la replicación, pero no se han excluido los discos.*  
  
## <a name="impact"></a>Impacto  
*Los archivos de paginación experimentan un gran volumen de actividad de entrada/salida, lo que requerirá innecesariamente recursos mucho mayores para participar en la replicación. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Si todavía no lo ha hecho, cree un disco duro virtual independiente para el archivo de paginación de Windows. Si ya se ha completado la replicación inicial, use el administrador de Hyper-V para quitar la replicación. A continuación, vuelva a configurar la replicación y excluya el disco duro virtual con el archivo de paginación de la replicación.*  
  


