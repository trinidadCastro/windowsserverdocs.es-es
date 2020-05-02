---
title: graftabl
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3815b85da163c03dea7bd2619d4647454d3ebd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724927"
---
# <a name="graftabl"></a>graftabl



Permite que los sistemas operativos Windows muestren un juego de caracteres extendido en el modo gráficos. Si se utiliza sin parámetros, **Graftabl** muestra las páginas de códigos anterior y actual.



## <a name="syntax"></a>Sintaxis

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Página de códigos>|Especifica una página de códigos para definir el aspecto de los caracteres extendidos en el modo gráficos.</br>Los números de identificación válidos de la página de códigos son:</br>437: Estados Unidos</br>850: multilingüe (Latín I)</br>852: Eslava (Latín II)</br>855: cirílico (Ruso)</br>857: Turco</br>860: Portugués</br>861: Islandés</br>863: Canadá-francés</br>865: Nordic</br>866: Ruso</br>869: Griego moderno|
|/status|Muestra la página de códigos actual que usa **Graftabl** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   **Graftabl** solo afecta a la presentación del monitor de caracteres extendidos de la página de códigos que especifique. No cambia la página de código de entrada de la consola real. Para cambiar la página de códigos de entrada de la consola, use el comando **mode** o **chcp** .
-   En la tabla siguiente se enumeran los códigos de salida y una breve descripción.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|El juego de caracteres se cargó correctamente. No se cargó ninguna página de códigos anterior.|
    |1|Se especificó un parámetro incorrecto. No se realizó ninguna acción.|
    |2|Error de archivo.|
-   Puede usar la variable de entorno ERRORLEVEL en un programa por lotes para procesar códigos de salida devueltos por **Graftabl**.

## <a name="examples"></a>Ejemplos

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