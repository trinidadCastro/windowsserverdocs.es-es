---
title: La autenticación basada en certificados está configurada, pero el certificado especificado no está instalado en el servidor réplica ni en los nodos del clúster de conmutación por error
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cc89aa201de4b25e4c221b770e6f88908785c859
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857688"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>La autenticación basada en certificados está configurada, pero el certificado especificado no está instalado en el servidor réplica ni en los nodos del clúster de conmutación por error

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema  
  
*El certificado de seguridad que la réplica de Hyper-V se ha configurado para usar para proporcionar replicación basada en certificados no está instalado en el servidor réplica (o en los nodos de clúster de conmutación por error).*  
  
## <a name="impact"></a>Impacto  
  
*En caso de que se realice una conmutación por error de clúster o se mueva a otro nodo, la replicación de Hyper-V se detendrá si el nuevo nodo no tiene también instalado el certificado adecuado. Esto afecta a los siguientes nodos:*  
  
\<lista de nodos >  
  
## <a name="resolution"></a>Resolución  
  
*Instale el certificado configurado en el servidor réplica (y todos los nodos asociados en el clúster de conmutación por error, si existe).*  
  


