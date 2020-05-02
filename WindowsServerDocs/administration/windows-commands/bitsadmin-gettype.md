---
title: bitsadmin gettype
description: Tema de referencia del comando bitsadmin GetType, que recupera el tipo de trabajo del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 151f9b8e81229a666111ebcd20f060d84160445a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717477"
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

#### <a name="output"></a>Output

Los valores de salida devueltos pueden ser:

| Tipo | Descripción |
| --------------- | ----------- |
| Descargar | El trabajo es una descarga. |
| Cargar | El trabajo es una carga. |
| Upload-Reply | El trabajo es una respuesta de carga. |
| Desconocido | El trabajo tiene un tipo desconocido. |

## <a name="examples"></a>Ejemplos

Para recuperar el tipo de trabajo para el trabajo denominado *myDownloadJob*:

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
