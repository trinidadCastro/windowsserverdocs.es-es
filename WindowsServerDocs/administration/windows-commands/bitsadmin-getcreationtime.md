---
title: bitsadmin getcreationtime
description: Artículo de referencia para el comando bitsadmin GetCreationTime, que recupera la hora de creación del trabajo especificado.
ms.topic: reference
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 780f7124248bd38f0c3d7cc9e7cb14b1a9decc03
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632232"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Recupera la hora de creación del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la hora de creación del trabajo denominado *myDownloadJob*:

```
bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
