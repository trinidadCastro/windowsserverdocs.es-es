---
title: bitsadmin getdescription
description: Artículo de referencia del comando bitsadmin getDescription, que recupera la descripción del trabajo especificado.
ms.topic: reference
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ddc3ef5f5f8328182e91ed7ae3026e94464267b7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632140"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

Recupera la descripción del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la descripción del trabajo denominado *myDownloadJob*:

```
bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
