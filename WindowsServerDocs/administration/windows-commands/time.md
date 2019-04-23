---
title: time
description: Obtenga información sobre cómo establecer y mostrar la hora del sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5c1f43be98a19c4b150c247cc7fd48d62edeb5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861916"
---
# <a name="time"></a>time



Muestra o establece la hora del sistema. Si se utiliza sin parámetros, **tiempo** muestra la hora actual del sistema y le pide que escriba un nuevo tiempo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<HH>[:\<MM>[:\<SS>[.\<NN>]]] [am\|pm]|Establece la hora del sistema en el nuevo horario especificado, donde *HH* es en horas (obligatorio), *MM* es en minutos, y *SS* es en segundos. *NN* puede utilizarse para especificar las centésimas de segundo. Si **estoy** o **pm** no se especifica, **tiempo** utiliza el formato de 24 horas de forma predeterminada.|
|/t|Muestra la hora actual sin pedir una nueva hora.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para cambiar la hora actual, debe tener credenciales administrativas.
-   Debe separar los valores de *HH*, *MM*, y *SS* con dos puntos (:). *SS* y *NN* deben separarse con un punto (.).
-   Válido *HH* los valores son 0 y 24.
-   Válido *MM* y *SS* los valores son de 0 a 59.

## <a name="BKMK_examples"></a>Ejemplos

Si se habilitan las extensiones de comando, para mostrar la hora actual del sistema, escriba:
```
time /t
```
Para cambiar la hora actual del sistema a 5:30 P.M., escriba cualquiera de las siguientes acciones:
```
time 17:30:00
time 5:30 pm
```
Para mostrar la hora del sistema actual, seguida por un símbolo del sistema que escriba una nueva hora, escriba:
```
The current time is: 17:33:31.35
Enter the new time:
```
Para mantener la hora actual y volver a la línea de comandos, presione ENTRAR. Para cambiar la hora actual, escriba la nueva hora y, a continuación, presione ENTRAR.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)