---
title: bitsadmin util y GETIEPROXY
description: Tema de comandos de Windows para **bitsadmin util y GETIEPROXY**, que recupera el uso de proxy para la cuenta de servicio especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22b24c4f9c0941c88c70b488a82de47c7901bd8b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122466"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util y GETIEPROXY

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera el uso de proxy para la cuenta de servicio especificada. Este comando muestra el valor de cada uso del proxy, no solo el uso del proxy especificado para la cuenta de servicio. Para obtener más información sobre cómo establecer el uso de proxy para cuentas de servicio específicas, vea el comando [bitsadmin util y SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| autorizada | Especifica la cuenta de servicio cuya configuración de proxy desea recuperar. Los valores posibles son:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| connectionName | Opcional. Se usa con el parámetro **/Conn** para especificar la conexión del módem que se va a usar. Si no se especifica el parámetro **/Conn** , bits usa la conexión LAN. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra el uso de proxy para la cuenta de servicio de red.

```
C:\>bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
