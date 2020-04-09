---
title: bitsadmin getnotifyinterface
description: Windows Commands topic for **bitsadmin getnotifyinterface**, que determina si otro programa ha registrado una interfaz de devolución de llamada com para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5eb5aee42446c70f16fd6785a3645f42c1987e4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850578"
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

#### <a name="output"></a>Salida

La salida de este comando muestra, **registrado** o **anulado el registro**.

> [!NOTE]
> No es posible determinar el programa que registró la interfaz de devolución de llamada.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la interfaz Notify del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)