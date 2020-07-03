---
title: bitsadmin getdescription
description: Artículo de referencia del comando bitsadmin getDescription, que recupera la descripción del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74e54f963325c0b7222dbd0f9bdccd44d0efc5e1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923061"
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
