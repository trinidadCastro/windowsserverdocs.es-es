---
title: Configurar el servidor con una cantidad suficiente de las direcciones MAC dinámicas
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fc444225c38ef7e8605ec328cfe3f8184b2fd307
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870736"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurar el servidor con una cantidad suficiente de las direcciones MAC dinámicas

>Se aplica a: Windows Server 2016

*En este tema está pensado para abordar un problema específico identificado por un análisis del analizador de procedimientos recomendados. Debe aplicar la información de este tema únicamente a los equipos que han tenido la ejecutó la herramienta Best Practices Analyzer de Hyper-V y que experimentan el problema mencionado en este tema. Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*El número de direcciones MAC dinámicas disponibles es bajo.*  
  
## <a name="impact"></a>Impacto  
  
*Cuando no hay direcciones MAC dinámicas están disponibles, no se puede iniciar las máquinas virtuales configuradas para usar una dirección MAC dinámica.*  
  
## <a name="resolution"></a>Resolución  
  
*Use el Administrador de conmutadores virtuales para ver y ampliar el alcance de las direcciones dinámicas.*  
  


