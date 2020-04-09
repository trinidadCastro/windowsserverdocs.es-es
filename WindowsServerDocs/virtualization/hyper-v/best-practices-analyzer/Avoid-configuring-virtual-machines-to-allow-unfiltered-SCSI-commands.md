---
title: Evite configurar máquinas virtuales para permitir comandos SCSI sin filtrar
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ac059bce1704a4e72b2c373d8186dbd4e31f2164
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857798"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evite configurar máquinas virtuales para permitir comandos SCSI sin filtrar

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Una máquina virtual está configurada para permitir comandos SCSI sin filtrar.*  
  
## <a name="impact"></a>Impacto  
  
*Omitir el filtrado de comandos SCSI supone un riesgo para la seguridad. Esta configuración solo debe habilitarse si es necesaria para la compatibilidad con las aplicaciones de almacenamiento que se ejecutan en el sistema operativo invitado. Las siguientes máquinas virtuales están configuradas para permitir comandos SCSI sin filtrar:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Póngase en contacto con el proveedor de almacenamiento para determinar si se requiere esta configuración. Además, si el sistema operativo de administración u otros sistemas operativos invitados están en peligro o presentan un comportamiento inusual, vuelva a configurar la máquina virtual para que bloquee los comandos.*  
  


