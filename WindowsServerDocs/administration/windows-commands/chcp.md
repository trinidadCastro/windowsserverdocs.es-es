---
title: chcp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 622d4b64128c7e39cc761e4f5e9d69cf54383760
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819206"
---
# <a name="chcp"></a>chcp



Cambia la página de códigos de la consola activa. Si se utiliza sin parámetros, **chcp** muestra el número de la página de códigos de la consola activa.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
chcp [<NNN>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NNN>|Especifica la página de códigos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

En la tabla siguiente se enumera cada admitidos en el código página y su país o región o idioma:

|Página de códigos|País o región o lenguaje|
|---------|--------------------------|
|437|Estados Unidos|
|850|Multilingüe (Latín I)|
|852|Eslavo (Latín II)|
|855|Cirílico (ruso)|
|857|Turco|
|860|Portugués|
|861|Islandés|
|863|Y el francés canadiense|
|865|Nordic|
|866|Ruso|
|869|Griego moderno|
|936|Chino|

## <a name="remarks"></a>Comentarios

-   Sólo la página de códigos de fabricante de equipos originales (OEM) que se instala con Windows aparece correctamente en una ventana del símbolo del sistema que utiliza fuentes Raster. Otras páginas de códigos aparecen correctamente en modo de pantalla completa o en las ventanas de símbolo del sistema que utilizan fuentes TrueType.
-   No es necesario preparar las páginas de códigos (como en MS-DOS).
-   Programas que se inicien después de asignan una nueva página de códigos utilizarán la nueva página de códigos. Sin embargo, programas (excepto Cmd.exe) que ha iniciado antes de asignan la página de código nuevo use la página de códigos original.

## <a name="BKMK_examples"></a>Ejemplos

Para ver la configuración de la página de código activo, escriba:
```
chcp
```
Aparece un mensaje similar al siguiente:

`Active code page: 437`

Para cambiar la página de códigos activa a 850 (multilingüe), escriba:
```
chcp 850
```
Si la página de códigos especificada no es válida, aparece el mensaje de error siguiente:

`Invalid code page`

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
