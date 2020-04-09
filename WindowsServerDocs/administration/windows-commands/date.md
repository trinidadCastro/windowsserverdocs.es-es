---
title: date
description: Comandos de Windows tema para Date, que muestra o establece la fecha del sistema. Si se usa sin parámetros,
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f9e32240eb27d651e324becefd72e9b1a545215
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846748"
---
# <a name="date"></a>date

Muestra o establece la fecha del sistema. Si se usa sin parámetros, **fecha** muestra la configuración actual de fecha del sistema y le pide que especifique una nueva fecha.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
date [/t | <Month-Day-Year>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<mes-día-año >|Establece la fecha especificada, donde *Month* es el mes (uno o dos dígitos), *Day* es el día (uno o dos dígitos) y *Year* es el año (dos o cuatro dígitos).|
|/t|Muestra la fecha actual sin pedir una nueva fecha.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para cambiar la fecha actual, debe tener credenciales administrativas.
-   Debe separar los valores de *mes*, *día*y *año* con puntos (.), guiones (-) o barras diagonales (/).
-   Los valores de *mes* válidos van de 1 a 12.
-   Los valores de *día* válidos van de 1 a 31.
-   Los valores de *año* válidos están comprendidos entre 00 y 99 o de 1980 a 2099. Si usa dos dígitos, los valores de 80 a 99 se corresponden con los años 1980 a 1999.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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