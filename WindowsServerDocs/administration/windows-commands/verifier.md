---
title: verifier
description: Artículo de referencia del comprobador, que ejecuta el administrador del comprobador de controladores.
ms.topic: reference
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1cdad5521d4ed6eb5b31bcbed9d09fd662200f71
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636321"
---
# <a name="verifier"></a>verifier

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Administrador del comprobador de controladores.

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
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<flags>|Debe ser un número en notación decimal o hexadecimal, combinación de bits:<p>-   **Valor: Descripción**<br />-   **bit 0:** comprobación de grupo especial<br />-   **bit 1:** forzar la comprobación de IRQL<br />-   **bit 2:** simulación de recursos insuficientes<br />-   **bit 3:** seguimiento del grupo<br />-   **bit 4:** Comprobación de e/s<br />-   **bit 5:** detección de interbloqueos<br />-   **bit 6:** sin usar<br />-   **bit 7:** Comprobación de DMA<br />-   **bit 8:** comprobaciones de seguridad<br />-   **bit 9:** forzar solicitudes de e/s pendientes<br />-   **bit 10:** Registro IRP<br />-   **bit 11:** comprobaciones varias<p>por ejemplo, **/Flags 27** es equivalente a **/Flags 0x1b**|
|/volatile|Se utiliza para cambiar la configuración del comprobador dinámicamente sin reiniciar el sistema. Cualquier nueva configuración se perderá cuando se reinicie el sistema.|
|\<probability>|Número comprendido entre 1 y 10.000 que especifica la probabilidad de inserción de errores. Por ejemplo, si se especifica 100, se indica una probabilidad de inserción de errores de 1% (100/10000).<p>Si no se especifica este parámetro, se utilizará la probabilidad predeterminada de 6%.|
|\<tags>|Especifica las etiquetas de grupo que se insertarán con errores, separados por caracteres de espacio. Si no se especifica este parámetro, se puede insertar cualquier asignación de grupo con errores.|
|\<applications>|Especifica el nombre del archivo de imagen de las aplicaciones que se insertarán con errores, separados por caracteres de espacio. Si no se especifica este parámetro, la simulación de recursos bajos puede tener lugar en cualquier aplicación.|
|\<minutes>|Un número positivo que especifica la duración del período después del reinicio, en minutos, durante el que no se producirá ninguna inserción de errores. Si no se especifica este parámetro, se usará la longitud predeterminada de 8 minutos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)