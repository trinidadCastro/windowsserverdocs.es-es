---
title: Se configura la autenticación basada en certificados, pero el certificado especificado no está instalado en el servidor de réplica o nodos de clúster de conmutación por error
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: feef30d2f798057e4bd8e53ebab240af6b1d25b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832946"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>Se configura la autenticación basada en certificados, pero el certificado especificado no está instalado en el servidor de réplica o nodos de clúster de conmutación por error

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  

En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.

## <a name="issue"></a>Problema  
  
*El certificado de seguridad que la réplica de Hyper-V se ha configurado para usar para proporcionar replicación basada en certificados no está instalada en el servidor réplica (o los nodos de clúster de conmutación por error).*  
  
## <a name="impact"></a>Impacto  
  
*Si se produce una conmutación por error de clúster o mover a otro nodo, replicación de Hyper-V se detendrá si el nuevo nodo no tiene instalado el certificado adecuado. Esto afecta a los nodos siguientes:*  
  
\<lista de nodos >  
  
## <a name="resolution"></a>Resolución  
  
*Instale el certificado configurado en el servidor réplica (y todos los nodos asociados en el clúster de conmutación por error, si existe).*  
  


