---
title: Una SAN virtual debe estar asociada a un adaptador de bus host físico
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 344b3b60c0d4613b121ef7ed17447810e0d6293d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965592"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Una SAN virtual debe estar asociada a un adaptador de bus host físico

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|


En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Se ha configurado una red de área de almacenamiento virtual (SAN) sin una asociación a un adaptador de bus host (HBA).*

## <a name="impact"></a>**Impacto**
*Una máquina virtual no se iniciará cuando esté configurada con un adaptador de Canal de fibra virtual conectado a una SAN Virtual mal configurada. Esto afecta a las siguientes San virtuales:*


\<list of virtual SANs>


## <a name="resolution"></a>**Resolución**
*Vuelva a configurar la SAN Virtual conectándola a un adaptador de bus host.*





