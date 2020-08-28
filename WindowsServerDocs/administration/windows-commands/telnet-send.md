---
title: telnet send
description: Artículo de referencia para el envío de Telnet, que envía comandos Telnet al servidor Telnet.
ms.topic: reference
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e8621d163e4f0bf7022f2cab1a67f188bc47dfd
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024589"
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
