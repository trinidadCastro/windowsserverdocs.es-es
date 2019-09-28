---
title: Use como mínimo la versión 3,0 del protocolo SMB para los recursos compartidos de archivos que almacenan archivos para máquinas virtuales.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: af23c3c860a47d0dd9096bc3f5ff466aca7836b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393324"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Use como mínimo la versión 3,0 del protocolo SMB para los recursos compartidos de archivos que almacenan archivos para máquinas virtuales.

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Los archivos de máquina virtual o los archivos de disco duro virtual se almacenan en un recurso compartido de archivos que no admite como mínimo la versión 3,0 del protocolo SMB.*  
  
## <a name="impact"></a>**Impacto**  
*Microsoft no admite esta configuración. Esto afecta a las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Mueva los archivos a un recurso compartido de archivos que use como mínimo la versión 3,0 del protocolo SMB.*  
  


