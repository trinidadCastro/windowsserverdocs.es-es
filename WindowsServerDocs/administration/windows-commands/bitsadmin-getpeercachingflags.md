---
title: bitsadmin getpeercachingflags
description: Tema de referencia para el comando bitsadmin getpeercachingflags, que recupera marcas que determinan si los archivos del trabajo pueden almacenarse en caché y servirse a los elementos del mismo nivel, y si BITS puede descargar contenido del trabajo desde los elementos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f30eead56958af3cd0fb0d91f6ee2bf9f79fdc4e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717690"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel, y si BITS puede descargar el contenido del trabajo de los elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar las marcas para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
