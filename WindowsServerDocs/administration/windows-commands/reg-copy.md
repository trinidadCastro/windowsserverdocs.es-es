---
title: reg copia
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11cbfce39433ea18632bec16c524da191def2a07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867566"
---
# <a name="reg-copy"></a>reg copia



Copia una entrada del registro en una ubicación especificada en el equipo local o remoto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName1>|Especifica la ruta de acceso completa de la subclave para copiar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<KeyName2>|Especifica la ruta de acceso completa de la subclave de destino. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/s|Copia todas las subclaves y entradas bajo la subclave especificada.|
|/f|Copia la subclave sin pedir confirmación.|
|/?|Muestra la Ayuda de **reg** copia en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Reg no pide confirmación al copiar una subclave.
-   En la tabla siguiente se enumera los valores devueltos por la **reg copia** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para copiar todas las subclaves y valores en la clave MyApp a la clave SaveMyApp, escriba:
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Para copiar todos los valores bajo la clave MyCo en el equipo llamado ZODIAC a la clave MyCo1 en el equipo actual, escriba:
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)