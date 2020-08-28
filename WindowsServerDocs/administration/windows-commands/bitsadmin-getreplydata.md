---
title: bitsadmin getreplydata
description: Artículo de referencia para el comando bitsadmin getreplydata, que recupera los datos de la respuesta de carga del servidor en formato hexadecimal para el trabajo.
ms.topic: reference
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e00a17e4633c9b065d03684cb0c953d1f6db811
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031343"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera los datos de la respuesta de carga del servidor en formato hexadecimal para el trabajo.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getreplydata <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar los datos de la respuesta de carga para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getreplydata myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
