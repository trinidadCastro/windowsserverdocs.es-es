---
title: Asegurarse de que el controlador de función virtual funciona correctamente cuando una máquina virtual está configurada para usar SR-IOV
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 282187d4d5a1243a14c3a0bdaa3088fe1f134bef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861928"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Asegurarse de que el controlador de función virtual funciona correctamente cuando una máquina virtual está configurada para usar SR-IOV

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
*El controlador de función virtual no funciona correctamente en el sistema operativo invitado de una o varias máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
*El rendimiento de red no es óptimo en las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*En el sistema operativo invitado, realice lo siguiente: Compruebe que estén instalados los controladores adecuados y que todos los dispositivos de red estén habilitados, y compruebe si hay errores o advertencias en el registro de eventos.*  
  


