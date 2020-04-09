---
title: Todos los adaptadores de red virtuales deben estar habilitados
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 450a3b42529be9a85991fcaf5263bae7b7827b1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857838"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Todos los adaptadores de red virtuales deben estar habilitados

>Se aplica a: Windows Server 2016


  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o más adaptadores de red virtuales asociados a un adaptador de red físico están deshabilitados en el sistema operativo de administración.*  
  
## <a name="impact"></a>Impacto  
  
*La configuración de este servidor no es óptima.*  
  
El sistema operativo de administración no se puede conectar a una red física (externa) mediante uno de los adaptadores de red físicos del equipo porque está asociado a un adaptador de red virtual deshabilitado.  
  
## <a name="resolution"></a>Resolución  
  
*Use la configuración de Internet de & de red para habilitar el adaptador de red virtual. O bien, use el administrador de conmutadores virtuales para volver a configurar el conmutador virtual externo de modo que no se comparta con el sistema operativo de administración.*  
  


