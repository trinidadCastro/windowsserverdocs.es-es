---
title: El número de procesadores lógicos en uso no debe superar el máximo admitido
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
ms.date: 8/16/2016
ms.openlocfilehash: 580d04af45416e08e536d815390be0e45b760312
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746150"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>El número de procesadores lógicos en uso no debe superar el máximo admitido

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Directiva|

En las secciones siguientes, cursiva indica el texto que aparece en la herramienta Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*El servidor está configurado con demasiados procesadores lógicos.*

## <a name="impact"></a>Impacto

*Microsoft no admite la ejecución de Hyper-V en este equipo.*

## <a name="resolution"></a>Solución

*Quite algunos procesadores de este equipo o use msconfig para limitar el número de procesadores disponibles.*

Vea las instrucciones siguientes para usar msconfig. Para obtener más información acerca de cómo quitar procesadores, consulte las instrucciones suministradas con el equipo o póngase en contacto con el fabricante del hardware. Para obtener más información acerca de las configuraciones máximas admitidas para Hyper-V, consulte [planear la escalabilidad de Hyper-v en Windows Server 2016](../plan/plan-hyper-v-scalability-in-windows-server.md).

### <a name="to-limit-the-number-of-available-processors"></a>Para limitar el número de procesadores disponibles

1.  Abra la aplicación de configuración del sistema (Msconfig.exe). Para ello, haga clic en **Inicio**, escriba **msconfig**, haga clic con el botón secundario en la aplicación de escritorio **configuración del sistema** y haga clic en **Ejecutar como administrador**.

2.  En la pestaña **arranque** , haga clic en **Opciones avanzadas**.

3.  Seleccione **número de procesadores** y, a continuación, seleccione un número en la lista. Haga clic en **Aceptar**.

4.  Reinicie el equipo para ejecutarlo con el nuevo número de procesadores.