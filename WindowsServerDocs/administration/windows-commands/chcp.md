---
title: chcp
description: Temas de comandos de Windows para chcp, que cambia la página de códigos de la consola activa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e644cf8544d135c5d21c344b0fd0a3364c7f89c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847948"
---
# <a name="chcp"></a>chcp

Cambia la página de códigos de la consola activa. Si se usa sin parámetros, **chcp** muestra el número de la página de códigos de la consola activa.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
chcp [<NNN>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NNN >|Especifica la página de códigos.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

En la tabla siguiente se enumeran todas las páginas de códigos admitidas y su país o región o idioma:

|Página de códigos|País o región o idioma|
|---------|--------------------------|
|437|Estados Unidos|
|850|Multilingüe (Latín I)|
|852|Eslava (Latín II)|
|855|Cirílico (Ruso)|
|857|Turco|
|860|Portugués|
|861|Islandés|
|863|Canadá (Francés)|
|865|Guantes|
|866|Ruso|
|869|Griego moderno|
|936|Chino|

## <a name="remarks"></a>Comentarios

-   Solo la página de códigos del fabricante de equipos originales (OEM) que se instala con Windows aparece correctamente en una ventana del símbolo del sistema que usa fuentes de tramas. Otras páginas de códigos aparecen correctamente en el modo de pantalla completa o en las ventanas del símbolo del sistema que usan fuentes TrueType.
-   No es necesario preparar las páginas de códigos (como en MS-DOS).
-   Los programas que se inician después de asignar una nueva página de códigos usan la nueva página de códigos. Sin embargo, los programas (excepto cmd. exe) que se inician antes de asignar la nueva página de códigos usan la página de códigos original.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver la configuración de la página de códigos activa, escriba:
```
chcp
```
Aparece un mensaje similar al siguiente:

`Active code page: 437`

Para cambiar la página de códigos activa a 850 (multilingüe), escriba:
```
chcp 850
```
Si la página de códigos especificada no es válida, aparece el siguiente mensaje de error:

`Invalid code page`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
