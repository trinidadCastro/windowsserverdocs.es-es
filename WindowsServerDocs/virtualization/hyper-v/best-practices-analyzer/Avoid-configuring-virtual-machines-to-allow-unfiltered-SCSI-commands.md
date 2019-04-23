---
title: Evite configurar máquinas virtuales para permitir que los comandos SCSI sin filtrar
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f401ce4d72f88d72529a95acea2a999df93679b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888276"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evite configurar máquinas virtuales para permitir que los comandos SCSI sin filtrar

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Una máquina virtual está configurada para permitir comandos SCSI sin filtrar.*  
  
## <a name="impact"></a>Impacto  
  
*Omitiendo el comando SCSI filtrado plantea un riesgo de seguridad. Esta configuración debe habilitarse solo si es necesario para la compatibilidad con aplicaciones de almacenamiento que se ejecutan en el sistema operativo invitado. Las siguientes máquinas virtuales se configuran para permitir que los comandos SCSI sin filtrar:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Póngase en contacto con el proveedor de almacenamiento para determinar si esta configuración es necesaria. Además, si el sistema operativo de administración o en otros sistemas operativos invitados están en peligro o un comportamiento inusual, vuelva a configurar la máquina virtual para bloquear los comandos.*  
  


