---
title: La autenticación basada en certificados está configurada, pero el certificado especificado no está instalado en el servidor réplica ni en los nodos del clúster de conmutación por error
description: Obtenga información acerca de qué hacer cuando el certificado de seguridad que la réplica de Hyper-V se ha configurado para usar para proporcionar replicación basada en certificados no está instalado en el servidor réplica (o en los nodos de clúster de conmutación por error).
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
ms.date: 8/16/2016
ms.openlocfilehash: 2f45c8e5699d8f55ea0ba0c69418217fd046bc63
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833630"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>La autenticación basada en certificados está configurada, pero el certificado especificado no está instalado en el servidor réplica ni en los nodos del clúster de conmutación por error

>Se aplica a: Windows Server 2016



*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*El certificado de seguridad que la réplica de Hyper-V se ha configurado para usar para proporcionar replicación basada en certificados no está instalado en el servidor réplica (o en los nodos de clúster de conmutación por error).*

## <a name="impact"></a>Impacto

*En caso de que se realice una conmutación por error de clúster o se mueva a otro nodo, la replicación de Hyper-V se detendrá si el nuevo nodo no tiene también instalado el certificado adecuado. Esto afecta a los siguientes nodos:*

\<list of nodes>

## <a name="resolution"></a>Solución

*Instale el certificado configurado en el servidor réplica (y todos los nodos asociados en el clúster de conmutación por error, si existe).*



