---
title: prompt
description: Artículo de referencia para el comando prompt, que personaliza el símbolo del sistema de Cmd.exe.
ms.topic: reference
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 8cf91a2a2e78191f035545bc897eceae4c897457
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640324"
---
# <a name="prompt"></a>prompt

Cambia la Cmd.exe símbolo del sistema, incluida la visualización del texto que desee, como el nombre del directorio actual, la fecha y la hora, o el número de versión de Microsoft Windows. Si se usa sin parámetros, este comando restablece el símbolo del sistema a la configuración predeterminada, que es la letra de unidad y el directorio actuales seguidos del símbolo mayor que ( **>** ).

## <a name="syntax"></a>Sintaxis

```
prompt [<text>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<text>` | Especifica el texto y la información que desea incluir en el símbolo del sistema. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Las combinaciones de caracteres que puede incluir en lugar de, o además de, una o más cadenas de caracteres en el parámetro de *texto* :

    | Carácter | Descripción |
    |--|--|
    | $q | = (Signo igual) |
    | $$ | $ (Signo de dólar) |
    | $t | Hora actual |
    | $d | Fecha actual |
    | $p | Unidad y ruta de acceso actuales |
    | $v | Número de versión de Windows |
    | $n | Unidad actual |
    | $g | > (mayor que signo) |
    | $l | < (menor que signo) |
    | $b | `|` (Símbolo de barra vertical) |
    | $_ | ENTRAR-AVANCE DE LA |
    | $e | Código de escape ANSI (código 27) |
    | $h | Retroceso (para eliminar un carácter que se ha escrito en la línea de comandos) |
    | $a | & (y comercial) |
    | $c | ((Paréntesis izquierdo) |
    | $f | ) (Paréntesis derecho) |
    | $s | Space |

- Cuando se habilitan las extensiones de comando, el comando **prompt** admite los siguientes caracteres de formato:

    | Carácter | Descripción |
    |--|--|
    | $+ | Cero o más caracteres de signo más ( **+** ), en función de la profundidad de la pila de directorios **insertada** (un carácter por cada nivel insertado). |
    | $m | Nombre remoto asociado a la letra de unidad actual o cadena vacía si la unidad actual no es una unidad de red. |

- Si incluye el carácter **$p** en el parámetro de texto, el disco se lee después de escribir cada comando (para determinar la unidad y la ruta de acceso actuales). Esto puede tardar más tiempo, especialmente en el caso de las unidades de disquete.

### <a name="examples"></a>Ejemplos

Para establecer un símbolo del sistema de dos líneas con la hora y fecha actuales en la primera línea y el signo mayor que en la línea siguiente, escriba:

```
prompt $d$s$s$t$_$g
```

El mensaje se cambia como se indica a continuación, donde la fecha y la hora son actuales:

```
Fri 06/01/2007  13:53:28.91
```

Para establecer que el símbolo del sistema se muestre como una flecha ( `-->` ), escriba:

```
prompt --$g
```

Para cambiar manualmente el símbolo del sistema a la configuración predeterminada (la unidad y la ruta de acceso actuales seguidos del signo mayor que), escriba:

```
prompt $p$g
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
