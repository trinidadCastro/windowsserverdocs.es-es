---
title: time
description: Obtenga información acerca de cómo establecer y mostrar la hora del sistema.
ms.topic: reference
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4faa12cef8485be6b76b16ff2ab545d0eae06cb7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640446"
---
# <a name="time"></a>time



Muestra o establece la hora del sistema. Si se usa sin parámetros, **Time** muestra la hora actual del sistema y le pide que especifique una nueva hora.



## <a name="syntax"></a>Sintaxis

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<HH>[:\<MM> [:\<SS> [.\<NN>]]] [AM \| PM]|Establece la hora del sistema en la nueva hora especificada, donde *HH* se encuentra en horas (obligatorio), *mm* es en minutos y *SS* está en segundos. *Nn* se puede usar para especificar centésimas de segundo. Si no se especifica **AM** o **PM** , **Time** usa el formato de 24 horas de forma predeterminada.|
|/t|Muestra la hora actual sin pedirle una nueva hora.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para cambiar la hora actual, debe tener credenciales administrativas.
-   Debe separar los valores de *HH*, *mm*y *SS* con dos puntos (:). *SS* y *nn* se deben separar con un punto (.).
-   Los valores de *HH* válidos son de 0 a 24.
-   Los valores válidos *mm* y *SS* son de 0 a 59.

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos

Si las extensiones de comando están habilitadas, para mostrar la hora actual del sistema, escriba:
```
time /t
```
Para cambiar la hora actual del sistema a 5:30 P.M., escriba una de las opciones siguientes:
```
time 17:30:00
time 5:30 pm
```
Para mostrar la hora del sistema actual, seguido de una solicitud para especificar una nueva hora, escriba:
```
The current time is: 17:33:31.35
Enter the new time:
```
Para mantener la hora actual y volver al símbolo del sistema, presione Entrar. Para cambiar la hora actual, escriba la nueva hora y, a continuación, presione Entrar.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)