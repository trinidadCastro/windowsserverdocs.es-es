---
title: Las máquinas virtuales configuradas con un adaptador de canal de fibra virtual se debe configurar para alta disponibilidad para el almacenamiento de canal de fibra
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 203477a022f7c5f819ef7b99f1b8e37a0b8b0a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816946"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Las máquinas virtuales configuradas con un adaptador de canal de fibra virtual se debe configurar para alta disponibilidad para el almacenamiento de canal de fibra

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Información de|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>**Problema**  
*Una o más máquinas virtuales no tienen una conexión al almacenamiento basado en canal de fibra de alta disponibilidad porque esas máquinas virtuales se configuran con un adaptador de canal de fibra virtual que está conectado al adaptador de bus de un solo host (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Un error en el adaptador de bus host podría bloquear la conexión de canal de fibra entre el almacenamiento y las máquinas virtuales. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Agregue otra conexión de la máquina virtual para el adaptador de bus host y configurar múltiples rutas (MPIO) de E/S en el sistema operativo invitado para establecer conexiones de canal de fibra redundantes.*  
  


