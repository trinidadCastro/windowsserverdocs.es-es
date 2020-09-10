---
title: telnet send
description: Artículo de referencia para el envío de Telnet, que envía comandos Telnet al servidor Telnet.
ms.topic: reference
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bef40b0015ccfdc5c62b6acc8b42bc95865ca0ff
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639811"
---
# <a name="telnet-send"></a>Telnet: enviar

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía comandos Telnet al servidor Telnet.

## <a name="syntax"></a>Sintaxis
```
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]
```
#### <a name="parameters"></a>Parámetros

| Parámetro |                     Descripción                      |
|-----------|------------------------------------------------------|
|    AO     |       Envía la salida de anulación del comando telnet.        |
|    ayt    |       Envía el comando telnet.       |
|    brk    |            Envía el comando telnet BRK.            |
|    esc    |      Envía el carácter de escape de Telnet actual.      |
|    ip     |     Envía el proceso de interrupción del comando de Telnet.     |
|   sincronización   |           Envía la sincronización de comandos de Telnet.           |
| <string>  | Envía cualquier cadena que escriba en el servidor Telnet. |
|     ?     |     Muestra la ayuda asociada a este comando.      |

## <a name="examples"></a>Ejemplos
Envíelo al servidor Telnet.
```
sen ayt
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
