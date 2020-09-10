---
title: bitsadmin cancel
description: Artículo de referencia del comando bitsadmin CANCEL, que quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados con el trabajo.
ms.topic: reference
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4ef8810b04141b41851f029f6cde4586b89a90d4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632421"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para quitar el trabajo *myDownloadJob* de la cola de transferencia:

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
