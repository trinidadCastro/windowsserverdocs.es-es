---
title: bitsadmin getnotifyinterface
description: Tema de referencia del comando bitsadmin getnotifyinterface, que determina si otro programa ha registrado una interfaz de devolución de llamada COM para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2158759067010292ca213f97014857354247b9c7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717737"
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
