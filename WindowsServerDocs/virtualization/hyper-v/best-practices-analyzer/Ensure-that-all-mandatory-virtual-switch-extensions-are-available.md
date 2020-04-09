---
title: Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 14c9fc31521a7d4f5e0eed821c0cc49dcfe74e69
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861938"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Uno o más adaptadores de red virtual están conectados a un conmutador virtual con extensiones obligatorias que están deshabilitadas o no instaladas.*  
  
## <a name="impact"></a>Impacto  
*El tráfico de red está bloqueado en uno o varios adaptadores de red virtual de las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*En primer lugar, asegúrese de que se ha instalado la extensión obligatoria en el host e instale la extensión si es necesario. A continuación, si la extensión obligatoria está deshabilitada, use el administrador de conmutadores virtuales o el cmdlet enable-VMSwitchExtension de Windows PowerShell para habilitar la extensión.*  
  


