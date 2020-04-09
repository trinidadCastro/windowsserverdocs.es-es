---
title: bitsadmin setcustomheaders
description: Windows Commands topic for bitsadmin setcustomheaders, que agrega un encabezado HTTP personalizado a una solicitud GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d97fae5f84637c80c3d1ef00aa36f09049bb17
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849618"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Agregue un encabezado HTTP personalizado a una solicitud GET.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Header1 Header2 . . .|Los encabezados personalizados para el trabajo|

## <a name="remarks"></a>Comentarios

-   Este modificador se usa para agregar un encabezado HTTP personalizado a una solicitud GET enviada a un servidor HTTP.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se agrega un encabezado HTTP personalizado para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob Accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)