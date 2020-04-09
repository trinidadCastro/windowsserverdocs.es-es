---
title: Una SAN virtual debe estar asociada a un adaptador de bus host físico
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 10a17f8661f1436f5b4db317648edde57a0dfe74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857938"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Una SAN virtual debe estar asociada a un adaptador de bus host físico

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Se ha configurado una red de área de almacenamiento virtual (SAN) sin una asociación a un adaptador de bus host (HBA).*  
  
## <a name="impact"></a>**Impacto**  
*Una máquina virtual no se iniciará cuando esté configurada con un adaptador de Canal de fibra virtual conectado a una SAN Virtual mal configurada. Esto afecta a las siguientes San virtuales:*  
  
  
\<lista de San virtuales >  
  
  
## <a name="resolution"></a>**Resolución**  
*Vuelva a configurar la SAN Virtual conectándola a un adaptador de bus host.*  
  
  
  


