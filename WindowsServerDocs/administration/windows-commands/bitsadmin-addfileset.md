---
title: bitsadmin addfileset
description: 'Temas de comandos de Windows para **bitsadmin addfileset** : agrega uno o varios archivos al trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 464f2da151d5a7bfffde286e52d9158560d48dcc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381989"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Agrega uno o más archivos al trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|TextFile|Un archivo de texto, cada línea de que contiene un nombre de archivo remoto y otro local.</br>Nota: Los nombres están delimitados por espacios. Las líneas que comienzan con un carácter # se tratan como comentario.|

## <a name="BKMK_examples"></a>Example

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)