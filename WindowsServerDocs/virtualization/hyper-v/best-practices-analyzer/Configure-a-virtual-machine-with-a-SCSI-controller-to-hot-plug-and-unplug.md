---
title: Configurar una máquina virtual con una controladora SCSI que se va a poder "hot" plug y desconecte frecuente almacenamiento
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 755e7485e54ee58e0acd7ebd75a7ee591aa655f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843286"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurar una máquina virtual con una controladora SCSI que se va a poder "hot" plug y desconecte frecuente almacenamiento

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Se encontró una máquina virtual que no está configurado con una controladora SCSI.*  
  
## <a name="impact"></a>Impacto  
  
*No podrá "hot" plug o desconecte frecuente almacenamiento para las máquinas virtuales siguientes:*  
  
\<lista de nombres de máquina virtual >  
  
La capacidad de conexión o desconectar frecuente almacenamiento resulta más fácil de administrar las necesidades de almacenamiento de una máquina virtual sin necesidad de tiempo de inactividad. Antes de que puede agregar o quitar el almacenamiento, se debe apagar las máquinas virtuales sin controladores SCSI.  
  
## <a name="resolution"></a>Resolución  
  
*Si no tiene conexión o desconectar frecuente almacenamiento para esta máquina virtual, se requiere ninguna acción. En caso contrario, apague la máquina virtual y agregue un controlador SCSI a la configuración.*  
  
Para usar un controlador SCSI conectar en caliente y desconecte frecuente almacenamiento, el sistema operativo invitado debe ejecutar la versión actual de integration services.  
  


