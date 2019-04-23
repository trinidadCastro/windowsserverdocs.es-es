---
title: El número de procesadores lógicos en uso no puede superar el máximo admitido
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d1275a17cc04494708f5ecfe9b708834b4233641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847076"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>El número de procesadores lógicos en uso no puede superar el máximo admitido

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Directiva|  
  
En las secciones siguientes, la cursiva indica texto que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*El servidor está configurado con demasiados procesadores lógicos.*  
  
## <a name="impact"></a>Impacto  
  
*Microsoft no es compatible con la que se ejecuta Hyper-V en este equipo.*  
  
## <a name="resolution"></a>Resolución  
  
*Quite algunos procesadores de esta máquina o usar msconfig para limitar el número de procesadores disponibles.*  
  
Consulte las instrucciones siguientes para usar Msconfig. Para obtener más información acerca de cómo quitar los procesadores, consulte las instrucciones suministradas con el equipo o póngase en contacto con el fabricante del hardware. Para obtener más información acerca de las configuraciones admitidas máximas para Hyper-V, consulte [planear la escalabilidad de Hyper-V en Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
### <a name="to-limit-the-number-of-available-processors"></a>Para limitar el número de procesadores disponibles  
  
1.  Abra el sistema de configuración de aplicación (Msconfig.exe). Para ello, haga clic en **iniciar**, tipo **msconfig**, haga clic en el **configuración del sistema** aplicación de escritorio y haga clic en **ejecutar como administrador**.  
  
2.  Desde el **arranque** , haga clic **opciones avanzadas**.  
  
3.  Seleccione **número de procesadores** y, a continuación, seleccione un número en la lista. Haga clic en **Aceptar**.  
  
4.  Reinicie el equipo para ejecutarlo mediante el nuevo número de procesadores.  
  


