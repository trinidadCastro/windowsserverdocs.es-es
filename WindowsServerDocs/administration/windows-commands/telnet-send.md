---
title: telnet send
description: Artículo de referencia para el comando telnet Send, que envía comandos Telnet al servidor Telnet.
ms.topic: reference
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ab6c94f619ab28be7b850479a7e3d476e1394e09
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717992"
---
# <a name="telnet-send"></a>Telnet: enviar

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía comandos Telnet al servidor Telnet.

## <a name="syntax"></a>Sintaxis

```
sen {ao | ayt | brk | esc | ip | synch | <string>} [?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| AO | Envía la salida de **anulación**del comando telnet. |
| ayt | ¿Envía el comando telnet **?** |
| brk | Envía el comando telnet **BRK**. |
| esc | Envía el carácter de escape de Telnet actual. |
| ip | Envía el proceso de **interrupción**del comando de Telnet. |
| sincronización | Envía la sincronización de comandos de Telnet. |
| `<string>` | Envía cualquier cadena que escriba en el servidor Telnet. |
| ? | Muestra la ayuda asociada a este comando. |

## <a name="example"></a>Ejemplo

Para enviar el comando **¿está ahí?** al servidor Telnet, escriba:

```
sen ayt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
