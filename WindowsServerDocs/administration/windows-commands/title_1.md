---
title: title
description: Windows Commands topic for title, que crea un título para la ventana del símbolo del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae18e97daaef226443c5f18c6c401a4e2b82b59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832808"
---
# <a name="title"></a>title

Crea un título para la ventana de símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
title [<String>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<cadena >|Especifica el título de la ventana del símbolo del sistema.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para crear el título de una ventana para los programas por lotes, incluya el comando **título** al principio de un programa por lotes.
-   Una vez establecido el título de una ventana, solo se puede restablecer mediante el comando **título** .

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el siguiente script de ejemplo, se cambia el título de la ventana del símbolo del sistema a actualizar archivos mientras el archivo por lotes ejecuta el comando **Copy** . Después de ejecutar el comando, se muestra el texto `Files Updated` y el título de la ventana del símbolo del sistema se vuelve a cambiar al símbolo del sistema.
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)