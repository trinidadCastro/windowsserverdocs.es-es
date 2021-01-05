---
title: Una SAN virtual debe estar asociada a un adaptador de bus host físico
description: Obtenga información acerca de qué hacer cuando se ha configurado una red de área de almacenamiento virtual (SAN) sin una asociación a un adaptador de bus host (HBA).
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
ms.date: 8/16/2016
ms.openlocfilehash: 73ce82a03e4831ab5056cb6e75c2bf08cc52b494
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834210"
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





