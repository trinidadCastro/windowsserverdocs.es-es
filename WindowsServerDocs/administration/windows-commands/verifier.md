---
title: verifier
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ab0833d4fdb11c4962d4916ec2e32097e08ca04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865876"
---
# <a name="verifier"></a>verifier

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administrador del Comprobador de controladores.  

## <a name="syntax"></a>Sintaxis  
```  
verifier /standard /driver <name> [<name> ...]  
verifier /standard /all  
verifier [/flags <flags>] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /driver <name> [<name>...]  
verifier [/flags FLAGS] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /all  
verifier /querysettings  
verifier /volatile /flags <flags>  
verifier /volatile /adddriver <name> [<name>...]  
verifier /volatile /removedriver <name> [<name>...]  
verifier /volatile /faults [<probability> [<tags> [<applications>]]  
verifier /reset  
verifier /query  
verifier /log <LogFileName> [/interval <seconds>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|\<flags>|Debe ser un número decimal o hexadecimal, combinación de bits:<br /><br />-   **Valor: descripción**<br />-   **bit 0:** comprobación de grupos especiales<br />-   **bit 1:** forzar comprobación irql<br />-   **bit 2:** bajo la simulación de recursos<br />-   **bit 3:** grupo de seguimiento<br />-   **bits 4:** Comprobación de E/S<br />-   **bit 5:** detección de interbloqueo<br />-   **bit 6:** sin usar<br />-   **bit 7:** Comprobación de DMA<br />-   **bit 8:** comprobaciones de seguridad<br />-   **bit 9:** exigir las solicitudes de E/S pendientes<br />-   **10 bits:** Registro de IRP<br />-   **11 de bits:** cheques varios<br /><br />Por ejemplo, **/flags 27** equivalente con **/flags 0x1B**|  
|/volatile|Se utiliza para cambiar la configuración del Comprobador dinámicamente sin necesidad de reiniciar el sistema. Cualquier nueva configuración se perderá cuando se reinicia el sistema.|  
|\<probability>|Número comprendido entre 1 y 10.000 que especifica la probabilidad de inyección de código de error. Por ejemplo, el valor de 100 significa una probabilidad de inyección de código de error de % 1 (100/10 000).<br /><br />Si no se especifica este parámetro, a continuación, se usará la probabilidad de valor predeterminado de % 6.|  
|\<tags>|Especifica las etiquetas de grupo que se insertará con errores, separados por caracteres de espacio. Si no se especifica este parámetro se puede insertar cualquier asignación de grupos con errores.|  
|\<applications>|Especifica el nombre de archivo de imagen de las aplicaciones que se insertará con errores, separados por caracteres de espacio. Si no se especifica este parámetro, a continuación, simulación de escasez de recursos puede realizar en cualquier aplicación.|  
|\<minutes>|Número positivo que especifica la longitud del período tras el reinicio, en minutos, durante los errores no se producirá por inyección de código. Si no se especifica este parámetro, a continuación, se utilizará la longitud predeterminada de 8 minutos.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  