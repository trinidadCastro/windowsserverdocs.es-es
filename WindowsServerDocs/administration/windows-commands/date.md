---
title: date
description: Artículo de referencia para el comando date, que muestra o establece la fecha del sistema. Si se usa sin parámetros,
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8966f02a6902b6b2bccc6fdc6931485a86bd39fe
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891513"
---
# <a name="date"></a>date

Muestra o establece la fecha del sistema. Si se usa sin parámetros, **fecha** muestra la configuración actual de fecha del sistema y le pide que especifique una nueva fecha.

>[!IMPORTANT]
> Para usar este comando, debe ser un administrador.

## <a name="syntax"></a>Sintaxis

```
date [/t | <month-day-year>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<month-day-year>` | Establece la fecha especificada, donde *Month* es el mes (uno o dos dígitos, incluidos los valores del 1 al 12), *Day* es el día (uno o dos dígitos, incluidos los valores del 1 al 31) y *Year* es el año (dos o cuatro dígitos, incluidos los valores del 00 al 99 o de 1980 a 2099). Debe separar los valores de *mes*, *día*y *año* con puntos (.), guiones (-) o barras diagonales (/).<p>**Nota:** Tenga en cuenta que si usa 2 dígitos para representar el año, los valores 80-99 corresponden a 1980 a 1999. |
| /t | Muestra la fecha actual sin pedir una nueva fecha. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Si las extensiones de comando están habilitadas, para mostrar la fecha actual del sistema, escriba:

```
date /t
```

Para cambiar la fecha actual del sistema al 3 de agosto de 2007, puede escribir cualquiera de las siguientes opciones:

```
date 08.03.2007
date 08-03-07
date 8/3/07
```

Para mostrar la fecha actual del sistema, seguida de una solicitud para escribir una nueva fecha, escriba:

```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yyyy)
```

Para mantener la fecha actual y volver al símbolo del sistema, presione **entrar**. Para cambiar la fecha actual, escriba la nueva fecha y, a continuación, presione **entrar**.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)