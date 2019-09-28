---
title: Los adaptadores de pantalla deben estar habilitados en las máquinas virtuales para proporcionar funcionalidades de vídeo
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0c515c7fb1ed160dfee1e1b7303022082e936157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364909"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Los adaptadores de pantalla deben estar habilitados en las máquinas virtuales para proporcionar funcionalidades de vídeo

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*El dispositivo de vídeo de bus de máquina virtual de Microsoft puede estar deshabilitado en una máquina virtual.*  
  
El dispositivo de vídeo de bus de máquina virtual de Microsoft es un adaptador de vídeo virtual optimizado para su uso con máquinas virtuales de Hyper-V. Cuando una máquina virtual no está configurada para usar el dispositivo de vídeo de bus de máquina virtual de Microsoft, se usa un adaptador de vídeo heredado. El dispositivo de vídeo de bus de máquina virtual de Microsoft funciona mejor que un adaptador de vídeo heredado.  
  
## <a name="impact"></a>Impacto  
  
*Se degradará el rendimiento de vídeo para las siguientes máquinas virtuales:*  
  
@no__t: 0list de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use Device Manager del sistema operativo invitado para habilitar el dispositivo de vídeo de bus de máquina virtual de Microsoft.*  
  
Los pasos necesarios para usar Device Manager varían en función del sistema operativo. Para obtener instrucciones, consulte la ayuda del sistema operativo invitado.  
  


