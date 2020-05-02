---
title: date
description: Tema de referencia de Date, que muestra o establece la fecha del sistema. Si se usa sin parámetros,
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7bcdb35579ac86b4ec7f9c7c639cf905f6a05fa
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716803"
---
# <a name="date"></a>date

Muestra o establece la fecha del sistema. Si se usa sin parámetros, **fecha** muestra la configuración actual de fecha del sistema y le pide que especifique una nueva fecha.



## <a name="syntax"></a>Sintaxis

```
date [/t | <Month-Day-Year>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Mes-día-año>|Establece la fecha especificada, donde *Month* es el mes (uno o dos dígitos), *Day* es el día (uno o dos dígitos) y *Year* es el año (dos o cuatro dígitos).|
|/t|Muestra la fecha actual sin pedir una nueva fecha.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para cambiar la fecha actual, debe tener credenciales administrativas.
-   Debe separar los valores de *mes*, *día*y *año* con puntos (.), guiones (-) o barras diagonales (/).
-   Los valores de *mes* válidos van de 1 a 12.
-   Los valores de *día* válidos van de 1 a 31.
-   Los valores de *año* válidos están comprendidos entre 00 y 99 o de 1980 a 2099. Si usa dos dígitos, los valores de 80 a 99 se corresponden con los años 1980 a 1999.

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
Enter the new date: (mm-dd-yy)
```
Para mantener la fecha actual y volver al símbolo del sistema, presione Entrar. Para cambiar la fecha actual, escriba la nueva fecha y, a continuación, presione Entrar.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)