---
title: Las entradas de autorización deben tener nombres de grupo de confianza distintos para los servidores principales con máquinas virtuales que no forman parte del mismo grupo de confianza
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c5b1a6bf8ef0bbceb5dde6b28cd951f399fc5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882966"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Las entradas de autorización deben tener nombres de grupo de confianza distintos para los servidores principales con máquinas virtuales que no forman parte del mismo grupo de confianza

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
*El servidor aceptará solicitudes de replicación para la máquina virtual de réplica desde cualquiera de los servidores en la lista de autorización asociado con la misma etiqueta de replicación que la máquina virtual.*  
  
## <a name="impact"></a>**Impact**  
*Puede haber privacidad y motivos de seguridad con una máquina virtual de aceptar la replicación de servidores principales que pertenecen a las entradas de autorización diferentes. Esto afecta a las siguientes entradas de autorización: \<lista de entradas de autorización >*  
  
## <a name="resolution"></a>**Resolución**  
*Usar etiquetas diferentes en las entradas de autorización para los servidores principales con máquinas virtuales que no forman parte del mismo grupo de seguridad. Modificar la configuración de Hyper-V para configurar las etiquetas de replicación.*  
  


