---
title: Configurar el servidor con una cantidad suficiente de direcciones MAC dinámicas
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1fe7b34abc8be9777a491a08016627e7b2a66a2a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862048"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurar el servidor con una cantidad suficiente de direcciones MAC dinámicas

>Se aplica a: Windows Server 2016

*El objetivo de este tema es resolver un problema específico identificado por un análisis de analizador de procedimientos recomendados. Debe aplicar la información de este tema únicamente a los equipos en los que se ha realizado Analizador de procedimientos recomendados de Hyper-V y que experimentan el problema que se trata en este tema. Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*El número de direcciones MAC dinámicas disponibles es bajo.*  
  
## <a name="impact"></a>Impacto  
  
*Cuando no hay ninguna dirección MAC dinámica disponible, no se pueden iniciar las máquinas virtuales configuradas para usar una dirección MAC dinámica.*  
  
## <a name="resolution"></a>Resolución  
  
*Use el administrador de conmutadores virtuales para ver y ampliar el intervalo de direcciones dinámicas.*  
  


