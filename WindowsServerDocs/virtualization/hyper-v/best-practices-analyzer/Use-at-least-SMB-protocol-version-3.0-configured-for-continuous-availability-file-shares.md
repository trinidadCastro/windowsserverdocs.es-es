---
title: Utilice al menos versión del protocolo SMB 3.0 configurada para la disponibilidad continua en recursos compartidos de archivos que almacenan los archivos para las máquinas virtuales
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67f41293433bd8d14096688fbaa23eb43334c738
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877876"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Utilice al menos versión del protocolo SMB 3.0 configurada para la disponibilidad continua en recursos compartidos de archivos que almacenan los archivos para las máquinas virtuales

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Archivos de máquina virtual o disco duro virtual se almacenan en un recurso compartido de red que no está configurado con la característica de disponibilidad continua de la versión del protocolo SMB 3.0.*  
  
## <a name="impact"></a>**Impact**  
*Microsoft no recomienda esta configuración porque puede afectar a la disponibilidad de las máquinas virtuales mediante el servidor. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
Realiza una de las siguientes acciones:  
  
-   Mover los archivos a un recurso compartido de archivos de SMB 3.0 que esté configurado para disponibilidad continua.  
  
-   Volver a configurar el recurso compartido de archivos actual para proporcionar disponibilidad continua.  
  


