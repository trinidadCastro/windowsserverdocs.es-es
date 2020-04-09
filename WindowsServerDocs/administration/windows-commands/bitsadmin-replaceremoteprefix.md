---
title: bitsadmin replaceremoteprefix
description: Windows Commands topic for **bitsadmin replaceremoteprefix**, que cambia la dirección URL remota para todos los archivos del trabajo de *oldprefix* a *newprefix*, según sea necesario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cea0108a292815e31e893e91dc4079305c1da9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849818"
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

En el ejemplo siguiente se cambia la dirección URL remota para todos los archivos del trabajo denominado *myDownloadJob*, desde *http://stageserver* a *http://prodserver* .

```
C:\>bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Información adicional

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)