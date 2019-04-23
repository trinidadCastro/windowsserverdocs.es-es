---
title: bitsadmin addfileset
description: Tema de los comandos de Windows para **addfileset bitsadmin** -agrega uno o varios archivos al trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8f6ff32dfa6042272c68647477d77183ce9cb76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889446"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Agrega uno o varios archivos al trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|TextFile|Un archivo de texto, cada línea de los cuales contiene un control remoto y un nombre de archivo local.</br>Nota: Los nombres están delimitados por espacios. Las líneas que comienzan por un carácter # se tratan como un comentario.|

## <a name="BKMK_examples"></a>Ejemplos

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)