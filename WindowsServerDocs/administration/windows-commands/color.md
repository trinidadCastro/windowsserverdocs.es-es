---
title: color
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8d23cc1bb5739b47c755d90c927cbcf82b8da7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825026"
---
# <a name="color"></a>color



Cambios en el primer y segundo plano los colores de la ventana de símbolo del sistema para la sesión actual. Si se utiliza sin parámetros, **color** restaura los colores de primer y segundo plano a la ventana de símbolo del sistema de predeterminado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<B>|Especifica el color de fondo.|
|\<F>|Especifica el color de primer plano.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   En la tabla siguiente se enumera los dígitos hexadecimales válidos que puede usar como valores para *B* y *F*.  
    |Valor|Color|
    |-----|-----|
    |0|Negro|
    |1|Azul|
    |2|Verde|
    |3|Aqua|
    |4|Roja|
    |5|Púrpura|
    |6|Amarillo|
    |7|Blanco|
    |8|Gris|
    |9|Azul claro|
    |A|Verde claro|
    |B|Aguamarina claro|
    |C|Rojo claro|
    |D|Púrpura claro|
    |E|Amarillo claro|
    |F|Blanco brillante|
-   No utilice caracteres de espacio entre *B* y *F*.
-   Si especifica solo un dígito hexadecimal, el color correspondiente se usa como el color de primer plano y el color de fondo se establece en el color predeterminado.
-   Para establecer el color predeterminado de la ventana de símbolo del sistema, haga clic en la esquina superior izquierda de la ventana de símbolo del sistema, haga clic en **valores predeterminados**, haga clic en el **colores** pestaña y, a continuación, haga clic en los colores que desee usar para el  **Texto de la pantalla** y **en segundo plano de pantalla**.
-   Si *B* y *F* son iguales, la **color** comando establece ERRORLEVEL a 1, y no se realiza ningún cambio en el primer plano o el color de fondo.

## <a name="BKMK_examples"></a>Ejemplos

Para cambiar el color de fondo de la ventana de símbolo del sistema a gris y el color de primer plano rojo, escriba:
```
color 84
```
Para cambiar el color de primer plano de la ventana de símbolo del sistema en amarillo claro, escriba:
```
color e
```

> [!NOTE]
> En este ejemplo, el fondo se establece en el color predeterminado porque se ha especificado un solo dígito hexadecimal.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)