---
title: bitsadmin replaceremoteprefix
description: Artículo de referencia para el comando bitsadmin replaceremoteprefix, que cambia la dirección URL remota para todos los archivos del trabajo de *oldprefix* a *newprefix*, según sea necesario.
ms.topic: reference
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 83c517a9126a3b78dfc919af5663e939aceee186
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631125"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Cambia la dirección URL remota para todos los archivos del trabajo de *oldprefix* a *newprefix*, según sea necesario.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| oldprefix | Prefijo de dirección URL existente. |
| newprefix | Nuevo prefijo de dirección URL. |

## <a name="examples"></a>Ejemplos

Para cambiar la dirección URL remota para todos los archivos del trabajo denominado *myDownloadJob*, de *http://stageserver* a *http://prodserver* .

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Información adicional

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
