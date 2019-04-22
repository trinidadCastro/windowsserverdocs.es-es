---
title: Asegúrese de que están disponibles todas las extensiones de conmutador virtual obligatorio
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 53ceeb9aab6ca7196454fbcd7f0fdae8b34d05d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825946"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Asegúrese de que están disponibles todas las extensiones de conmutador virtual obligatorio

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Uno o más adaptadores de red virtual están conectados a un conmutador virtual con extensiones obligatorios que están deshabilitados o no está instalado.*  
  
## <a name="impact"></a>Impacto  
*Se bloquea el tráfico de red en uno o más adaptadores de red virtual en las máquinas virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*En primer lugar, asegúrese de que se ha instalado la extensión obligatoria en el host e instalar la extensión si es necesario. A continuación, si se deshabilita la extensión obligatoria, utilice Administrador de conmutadores virtuales o el cmdlet Enable-VMSwitchExtension de Windows PowerShell para habilitar la extensión.*  
  


