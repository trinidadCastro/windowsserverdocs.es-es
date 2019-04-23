---
title: date
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e774f7bfabb9b574255691dd97d2cfff36f034e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877956"
---
# <a name="date"></a>date



Muestra o establece la fecha del sistema. Si se utiliza sin parámetros, **fecha** muestra la configuración de fecha del sistema actual y le pide que escriba una nueva fecha.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Mes-año >|Establece la fecha especificada, donde *mes* es el mes (uno o dos dígitos) *día* es el día (uno o dos dígitos) y *año* es el año (dos o cuatro dígitos).|
|/t|Muestra la fecha actual sin pedir una nueva fecha.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para cambiar la fecha actual, debe tener credenciales administrativas.
-   Debe separar los valores de *mes*, *día*, y *año* con puntos (.), guiones (-) o una barra diagonal (/) de marca.
-   Válido *mes* valores son de 1 a 12.
-   Válido *día* los valores son 1 y 31.
-   Válido *año* valores están comprendidos entre 00 y 99, o bien 1980 a 2099. Si usa dos dígitos, los valores de 80 a 99 se corresponden con los años de 1980 a 1999.

## <a name="BKMK_examples"></a>Ejemplos

Si se habilitan las extensiones de comando, para mostrar la fecha actual del sistema, escriba:
```
date /t
```
Para cambiar la fecha actual del sistema para el 3 de agosto de 2007, puede escribir cualquiera de las siguientes acciones:
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
Para mostrar la fecha del sistema actual, seguida por un símbolo del sistema para especificar una fecha nueva, escriba:
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
Para mantener la fecha actual y volver a la línea de comandos, presione ENTRAR. Para cambiar la fecha actual, escriba la nueva fecha y, a continuación, presione ENTRAR.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)