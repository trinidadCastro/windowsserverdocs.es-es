---
title: bitsadmin util y setieproxy
description: Artículo de referencia del comando bitsadmin util y SETIEPROXY, que establece la configuración del proxy que se va a usar al transferir archivos mediante una cuenta de servicio.
ms.topic: reference
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7c341a3fffad0951b800c618605c15b47d4d9308
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630423"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util y setieproxy

Establezca la configuración de proxy que se va a usar al transferir archivos mediante una cuenta de servicio. Debe ejecutar este comando desde un símbolo del sistema con privilegios elevados para que se complete correctamente.

> [!NOTE]
> Este comando no es compatible con BITS 1,5 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /setieproxy <account> <usage> [/conn <connectionname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| account | Especifica la cuenta de servicio cuya configuración de proxy desea definir. Los valores posibles son:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| usage | Especifica la forma de detección del proxy que se va a usar. Los valores posibles son:<ul><li>**NO_PROXY.** No utilice un servidor proxy.</li><li>**Detección automática.** Detectar automáticamente la configuración de proxy.</li><li>**MANUAL_PROXY.** Usar una lista de proxy especificada y una lista de omisión. Debe especificar las listas inmediatamente después de la etiqueta Usage. Por ejemplo, `MANUAL_PROXY proxy1,proxy2 NULL`.<ul><li>**Lista de proxy.** Una lista delimitada por comas de servidores proxy que se usarán.</li><li>**Omitir lista.** Una lista delimitada por espacios de nombres de host o direcciones IP, o ambos, para las que las transferencias no se enrutarán a través de un proxy. Esto puede ser \<local> hacer referencia a todos los servidores de la misma LAN. Los valores NULL o se pueden usar para una lista de omisión de proxy vacía.</li></ul><li>**Autoscript.** Igual que la **detección automática**, excepto en que también se ejecuta un script. Debe especificar la dirección URL del script inmediatamente después de la etiqueta Usage. Por ejemplo, `AUTOSCRIPT http://server/proxy.js`.</li><li>**Determinado.** Igual que **NO_PROXY**, excepto que quita las direcciones URL de proxy manuales (si se especifican) y las direcciones URL detectadas mediante la detección automática.</li></ul> |
| connectionName | Opcional. Se usa con el parámetro **/Conn** para especificar la conexión del módem que se va a usar. Si no se especifica el parámetro **/Conn** , bits usa la conexión LAN. |

### <a name="remarks"></a>Observaciones

Cada llamada sucesiva con este modificador reemplaza el uso previamente especificado, pero no los parámetros del uso definido previamente. Por ejemplo, si especifica **NO_PROXY**, **detección automática**y **MANUAL_PROXY** en llamadas independientes, bits usa el último uso proporcionado, pero conserva los parámetros del uso definido previamente.

## <a name="examples"></a>Ejemplos

Para establecer el uso de proxy para la cuenta LOCALSYSTEM:

```
bitsadmin /util /setieproxy localsystem AUTODETECT
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin util (comando)](bitsadmin-util.md)

- [bitsadmin (comando)](bitsadmin.md)
