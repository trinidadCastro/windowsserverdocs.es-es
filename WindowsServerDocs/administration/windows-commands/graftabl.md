---
title: graftabl
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d55df814cb962e82775a86e154a024c579987cf2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842418"
---
# <a name="graftabl"></a>graftabl



Permite que los sistemas operativos Windows muestren un juego de caracteres extendido en el modo gráficos. Si se utiliza sin parámetros, **Graftabl** muestra las páginas de códigos anterior y actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<página de códigos >|Especifica una página de códigos para definir el aspecto de los caracteres extendidos en el modo gráficos.</br>Los números de identificación válidos de la página de códigos son:</br>437: Estados Unidos</br>850: multilingüe (Latín I)</br>852: Eslava (Latín II)</br>855: cirílico (Ruso)</br>857: Turco</br>860: Portugués</br>861: Islandés</br>863: Canadá-francés</br>865: Nordic</br>866: Ruso</br>869: Griego moderno|
|/status|Muestra la página de códigos actual que usa **Graftabl** .|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   **Graftabl** solo afecta a la presentación del monitor de caracteres extendidos de la página de códigos que especifique. No cambia la página de código de entrada de la consola real. Para cambiar la página de códigos de entrada de la consola, use el comando **mode** o **chcp** .
-   En la tabla siguiente se enumeran los códigos de salida y una breve descripción.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|El juego de caracteres se cargó correctamente. No se cargó ninguna página de códigos anterior.|
    |1|Se especificó un parámetro incorrecto. No se realizó ninguna acción.|
    |2|Error de archivo.|
-   Puede usar la variable de entorno ERRORLEVEL en un programa por lotes para procesar códigos de salida devueltos por **Graftabl**.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver la página de códigos actual utilizada por **Graftabl**, escriba:
```
graftabl /status
```
Para cargar el juego de caracteres gráficos de la página de códigos 437 (Estados Unidos) en la memoria, escriba:
```
graftabl 437
```
Para cargar el juego de caracteres gráficos de la página de códigos 850 (multilingüe) en la memoria, escriba:
```
graftabl 850
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Chcp](chcp.md)