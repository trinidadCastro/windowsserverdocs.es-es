---
title: La configuración de PVLAN en un conmutador virtual debe ser coherente
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7e42c874e883157b3f85f523511b950e2863b155
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861858"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>La configuración de PVLAN en un conmutador virtual debe ser coherente

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.
  
## <a name="issue"></a>**Problema**  
*La red de área local virtual privada (PVLAN) no está configurada correctamente en uno o más adaptadores de red virtuales.*  
  
## <a name="impact"></a>**Impacto**  
*Es posible que PVLAN no Aísle correctamente el tráfico de red entre las máquinas virtuales.*  
  
## <a name="resolution"></a>**Resolución**  
*Use el cmdlet de Windows PowerShell, Set-VMNetworkAdapterVlan, para configurar PVLAN correctamente.*  
  


