---
title: Use como mínimo la versión 3,0 del protocolo SMB configurado para disponibilidad continua en recursos compartidos de archivos que almacenan archivos para máquinas virtuales.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 18943c6b34ab74206483779db5afa06bbde04874
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854208"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Use como mínimo la versión 3,0 del protocolo SMB configurado para disponibilidad continua en recursos compartidos de archivos que almacenan archivos para máquinas virtuales.

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
*Los archivos de máquina virtual o los archivos de disco duro virtual se almacenan en un recurso compartido de archivos de red que no está configurado con la característica de disponibilidad continua de la versión 3,0 del protocolo SMB.*  
  
## <a name="impact"></a>**Impacto**  
*Microsoft no recomienda esta configuración porque podría afectar a la disponibilidad de las máquinas virtuales con el servidor. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
Realice una de las siguientes acciones:  
  
-   Mueva los archivos a un recurso compartido de archivos SMB 3,0 que esté configurado para disponibilidad continua.  
  
-   Vuelva a configurar el recurso compartido de archivos actual para proporcionar disponibilidad continua.  
  


