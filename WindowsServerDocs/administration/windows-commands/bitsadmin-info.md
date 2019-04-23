---
title: bitsadmin info
description: Tema de los comandos de Windows para **muestra información resumida sobre el trabajo especificado.** -info bitsadmin
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ee96c69e311600a53f04b1b883983718adf0f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851526"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Muestra información resumida sobre el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Use el / verbose parámetro para proporcionar información detallada sobre el trabajo.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera información sobre el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)