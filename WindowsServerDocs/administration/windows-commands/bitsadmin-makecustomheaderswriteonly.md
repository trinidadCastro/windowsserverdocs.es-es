---
title: bitsadmin makecustomheaderswriteonly
description: Artículo de referencia para el comando bitsadmin makecustomheaderswriteonly, que hace que los encabezados HTTP personalizados de un trabajo sean de solo escritura.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0eee6cc2fd8c825f02f7e01750be743b10beb73e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631484"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Hacer que los encabezados HTTP personalizados de un trabajo sean de solo escritura.

> [!IMPORTANT]
> Esta acción no se puede deshacer.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para hacer que los encabezados HTTP personalizados sean de solo escritura para el trabajo denominado *myDownloadJob*:

```
bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
