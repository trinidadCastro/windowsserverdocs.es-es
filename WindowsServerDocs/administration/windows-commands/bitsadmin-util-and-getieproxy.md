---
title: bitsadmin util y getieproxy
description: Artículo de referencia del comando bitsadmin util y GETIEPROXY, que recupera el uso del proxy para la cuenta de servicio determinada.
ms.topic: reference
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c10fa442f19ff7d5de44e12986b8af4be3f39e19
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630495"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util y getieproxy

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera el uso de proxy para la cuenta de servicio especificada. Este comando muestra el valor de cada uso del proxy, no solo el uso del proxy especificado para la cuenta de servicio. Para obtener más información sobre cómo establecer el uso de proxy para cuentas de servicio específicas, vea el comando [bitsadmin util y SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| account | Especifica la cuenta de servicio cuya configuración de proxy desea recuperar. Los valores posibles son:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| connectionName | Opcional. Se usa con el parámetro **/Conn** para especificar la conexión del módem que se va a usar. Si no se especifica el parámetro **/Conn** , bits usa la conexión LAN. |

## <a name="examples"></a>Ejemplos

Para mostrar el uso de proxy para la cuenta de servicio de red:

```
bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin util (comando)](bitsadmin-util.md)

- [bitsadmin (comando)](bitsadmin.md)
