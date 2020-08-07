---
title: bitsadmin getnotifyinterface
description: Artículo de referencia del comando bitsadmin getnotifyinterface, que determina si otro programa ha registrado una interfaz de devolución de llamada COM para el trabajo especificado.
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0e22b240f16aa417cf46b715527cd9098c60794
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894060"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina si otro programa ha registrado una interfaz de devolución de llamada COM (la interfaz de notificación) para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

#### <a name="output"></a>Output

La salida de este comando muestra, **registrado** o **anulado el registro**.

> [!NOTE]
> No es posible determinar el programa que registró la interfaz de devolución de llamada.

## <a name="examples"></a>Ejemplos

Para recuperar la interfaz Notify del trabajo denominado *myDownloadJob*:

```
bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
