---
title: title
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 42094e0f1231fee5ac9ef0ec9184ba685c8846b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385787"
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
|\<cadena >|Especifica el título de la ventana del símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para crear el título de una ventana para los programas por lotes, incluya el comando **título** al principio de un programa por lotes.
-   Una vez establecido el título de una ventana, solo se puede restablecer mediante el comando **título** .

## <a name="BKMK_examples"></a>Example

En el siguiente script de ejemplo, el título de la ventana del símbolo del sistema cambia a "actualizando archivos", mientras que el archivo por lotes ejecuta el comando **Copy** . Después de ejecutar el comando, se muestra el texto `Files Updated` y el título de la ventana del símbolo del sistema se vuelve a cambiar a "símbolo del sistema".
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)