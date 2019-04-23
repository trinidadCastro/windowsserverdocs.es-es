---
title: Bitsadmin setcustomheaders
description: Tema de los comandos de Windows para **setcustomheaders bitsadmin** -agregar un encabezado HTTP personalizado a una solicitud GET.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d90ac2d23b852ae0c2114e7cd5a9c9e6382ce8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853856"
---
# <a name="bitsadmin-setcustomheaders"></a>Bitsadmin setcustomheaders



Agregar un encabezado HTTP personalizado a una solicitud GET.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Header1 2. . .|Los encabezados personalizados para el trabajo|

## <a name="remarks"></a>Comentarios

-   Este modificador se usa para agregar un encabezado HTTP personalizado a una solicitud GET que se envían a un servidor HTTP.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se agrega un encabezado HTTP personalizado para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)