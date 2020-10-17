---
title: time
description: Artículo de referencia del comando Time, que muestra o establece la hora del sistema.
ms.topic: reference
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b69db2fce1af52d8f284e79fe83f5ac20c398cd6
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156474"
---
# <a name="time"></a>time

Muestra o establece la hora del sistema. Si se usa sin parámetros, **Time** muestra la hora actual del sistema y le pide que especifique una nueva hora.

> [!NOTE]
> Debe ser un administrador para cambiar la hora actual.

## <a name="syntax"></a>Sintaxis

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<HH>[:<MM>[:<SS>[.<NN>]]] [am | pm]` | Establece la hora del sistema en la nueva hora especificada, donde *HH* se encuentra en horas (obligatorio), *mm* es en minutos y *SS* está en segundos. *Nn* se puede usar para especificar centésimas de segundo. Debe separar los valores de *HH*, *mm*y *SS* con dos puntos (:). *SS* y *nn* se deben separar con un punto (.).<p>Si no se especifica **AM** o **PM** , **Time** usa el formato de 24 horas de forma predeterminada. |
| /t | Muestra la hora actual sin pedirle una nueva hora. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Los valores de *HH* válidos son de 0 a 24.

- Los valores válidos *mm* y *SS* son de 0 a 59.

## <a name="examples"></a>Ejemplos

Si las extensiones de comando están habilitadas, para mostrar la hora actual del sistema, escriba:

```
time /t
```

Para cambiar la hora actual del sistema a 5:30 PM, escriba una de las opciones siguientes:

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
