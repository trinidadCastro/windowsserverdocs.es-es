---
title: bitsadmin gettype
description: Artículo de referencia del comando bitsadmin GetType, que recupera el tipo de trabajo del trabajo especificado.
ms.topic: reference
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ccb938e1fe67164567bbe8d6c87d87423026db96
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631563"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Recupera el tipo de trabajo del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

#### <a name="output"></a>Resultados

Los valores de salida devueltos pueden ser:

| Tipo | Descripción |
| --------------- | ----------- |
| Descargar | El trabajo es una descarga. |
| Cargar | El trabajo es una carga. |
| Upload-Reply | El trabajo es una respuesta de carga. |
| Unknown | El trabajo tiene un tipo desconocido. |

## <a name="examples"></a>Ejemplos

Para recuperar el tipo de trabajo para el trabajo denominado *myDownloadJob*:

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
