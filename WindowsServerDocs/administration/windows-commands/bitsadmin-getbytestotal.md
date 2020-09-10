---
title: bitsadmin getbytestotal
description: Artículo de referencia para el comando bitsadmin getbytestotal, que recupera el tamaño del trabajo especificado.
ms.topic: reference
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 50b926b8d89e402ef4fd9e58896963edd3c368c9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632347"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera el tamaño del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el tamaño del trabajo denominado *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
