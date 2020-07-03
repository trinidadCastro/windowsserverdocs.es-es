---
title: bitsadmin replaceremoteprefix
description: Artículo de referencia para el comando bitsadmin replaceremoteprefix, que cambia la dirección URL remota para todos los archivos del trabajo de *oldprefix* a *newprefix*, según sea necesario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2453ac4c223baa049980578c81d9bc6539baac7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927973"
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
