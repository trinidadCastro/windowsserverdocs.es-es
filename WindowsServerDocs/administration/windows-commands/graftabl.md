---
title: graftabl
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395873cf3dbeb574dd9abc69f45b410bece80c25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848446"
---
# <a name="graftabl"></a>graftabl



Permite a los sistemas operativos de Windows mostrar un carácter extendido en modo gráfico. Si se utiliza sin parámetros, **graftabl** muestra anterior y la página de códigos actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
graftabl <CodePage>
graftabl /status
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<CodePage>|Especifica una página de códigos para definir la apariencia de los caracteres extendidos en el modo de gráficos.</br>Números de identificación de página de código válidos son:</br>437: Estados Unidos</br>850: Multilingüe (Latín I)</br>852: Eslavo (Latín II)</br>855: Cirílico (ruso)</br>857: Turco</br>860: Portugués</br>861: Islandés</br>863: Y el francés canadiense</br>865: Nordic</br>866: Ruso</br>869: Griego moderno|
|/status|Muestra la actual página de códigos que **graftabl** está usando.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   **Graftabl** afecta solo la pantalla del monitor de caracteres extendidos de la página de códigos que especifique. No se cambia la página de códigos de entrada de consola real. Para cambiar la página de códigos de entrada de consola, use el **modo** o **chcp** comando.
-   En la tabla siguiente se enumera los códigos de salida y una breve descripción del mismo.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|Juego de caracteres se cargó correctamente. No hay ninguna página de código anterior se cargó.|
    |1|Se especificó un parámetro incorrecto. No se realizó ninguna acción.|
    |2|Se produjo un error de archivo.|
-   Puede usar la variable de entorno ERRORLEVEL en un programa por lotes para procesar los códigos de salida devueltos por **graftabl**.

## <a name="BKMK_examples"></a>Ejemplos

Para ver la página de códigos actual utilizada por **graftabl**, tipo:
```
graftabl /status
```
Para cargar el conjunto de caracteres gráficos para la página de códigos 437 (Estados Unidos) en la memoria, escriba:
```
graftabl 437
```
Para cargar los gráficos de caracteres establecido para la página de códigos 850 (multilingual) en la memoria, escriba:
```
graftabl 850
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Chcp](chcp.md)