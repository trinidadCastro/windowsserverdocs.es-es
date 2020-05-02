---
title: bitsadmin replaceremoteprefix
description: Tema de referencia del comando bitsadmin replaceremoteprefix, que cambia la dirección URL remota para todos los archivos del trabajo de *oldprefix* a *newprefix*, según sea necesario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 745d026513413db799e86df3422d5ee19c89274f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717037"
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

Para cambiar la dirección URL remota para todos los archivos del *myDownloadJob*trabajo denominado myDownloadJob *http://stageserver* , *http://prodserver*de a.

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Información adicional

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
