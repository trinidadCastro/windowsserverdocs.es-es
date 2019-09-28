---
title: bitsadmin setcustomheaders
description: Temas de comandos de Windows para **bitsadmin setcustomheaders** -agregar un encabezado HTTP personalizado a una solicitud GET.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45e3a5178df69b84618966ca0fcd9cc1e6d0e449
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380643"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders



Agregue un encabezado HTTP personalizado a una solicitud GET.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Header1 Header2 . . .|Los encabezados personalizados para el trabajo|

## <a name="remarks"></a>Comentarios

-   Este modificador se usa para agregar un encabezado HTTP personalizado a una solicitud GET enviada a un servidor HTTP.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se agrega un encabezado HTTP personalizado para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)