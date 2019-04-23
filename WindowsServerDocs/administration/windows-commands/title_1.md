---
title: title
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1d1ea70849c3beb4503edfdaa5116384c14a2fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848506"
---
# <a name="title"></a>title



Crea un título para la ventana de símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
title [<String>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<String>|Especifica el título de la ventana de símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para crear el título de la ventana programas por lotes, incluya el **título** comando al principio de un programa por lotes.
-   Después de establece un título de ventana, puede restablecer solo mediante la **título** comando.

## <a name="BKMK_examples"></a>Ejemplos

En el siguiente script de ejemplo, el título de la ventana de símbolo del sistema se cambia a "Archivos de actualización" mientras se ejecuta el archivo por lotes la **copia** comando. Después de ejecuta el comando, el texto `Files Updated` se muestra, y el título de la ventana de símbolo del sistema vuelve a cambiar a "Símbolo".
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)