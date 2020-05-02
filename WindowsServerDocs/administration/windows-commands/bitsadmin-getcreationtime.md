---
title: bitsadmin getcreationtime
description: Tema de referencia del comando bitsadmin GetCreationTime, que recupera la hora de creación del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc6ca5ad23730e9f57d58e069e0a2daf961930e8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718104"
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
