---
title: copiar reg
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a82b17b631d4242fa6affdec0ff67b5b09380550
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371778"
---
# <a name="reg-copy"></a>copiar reg



Copia una entrada del registro en una ubicación especificada en el equipo local o remoto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0KeyName1 >|Especifica la ruta de acceso completa de la subclave que se va a copiar. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\ @ no__t-1ComputerName @ no__t-2 como parte del nombre de *clave*. Si se omite \\ @ no__t-1ComputerName, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|@no__t 0KeyName2 >|Especifica la ruta de acceso completa del destino de la subclave. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\ @ no__t-1ComputerName @ no__t-2 como parte del nombre de *clave*. Si se omite \\ @ no__t-1ComputerName, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/s|Copia todas las subclaves y entradas de la subclave especificada.|
|/f|Copia la subclave sin pedir confirmación.|
|/?|Muestra la ayuda de la copia **reg** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Reg no pide confirmación al copiar una subclave.
-   En la tabla siguiente se enumeran los valores devueltos para la operación de **copia de reg** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para copiar todas las subclaves y valores de la clave MyApp en la clave SaveMyApp, escriba:
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Para copiar todos los valores de la clave MyCo del equipo denominado zodíaco en la clave MyCo1 del equipo actual, escriba:
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)