---
title: title
description: Artículo de referencia del título, que crea un título para la ventana del símbolo del sistema.
ms.topic: reference
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1160326d2627b62da120e364941627b64730f721
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640429"
---
# <a name="title"></a>title

Crea un título para la ventana de símbolo del sistema.



## <a name="syntax"></a>Sintaxis

```
title [<String>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<String>|Especifica el título de la ventana del símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para crear el título de una ventana para los programas por lotes, incluya el comando **título** al principio de un programa por lotes.
-   Una vez establecido el título de una ventana, solo se puede restablecer mediante el comando **título** .

## <a name="examples"></a>Ejemplos

En el siguiente script de ejemplo, se cambia el título de la ventana del símbolo del sistema a actualizar archivos mientras el archivo por lotes ejecuta el comando **Copy** . Después de ejecutar el comando, `Files Updated` se muestra el texto y el título de la ventana del símbolo del sistema se vuelve a cambiar al símbolo del sistema.
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)