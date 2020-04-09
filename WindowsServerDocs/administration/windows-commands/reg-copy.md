---
title: copiar reg
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2acfdd3c0ad66d93313a11f8025b690ea0157c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836538"
---
# <a name="reg-copy"></a>copiar reg



Copia una entrada del registro en una ubicación especificada en el equipo local o remoto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName1 >|Especifica la ruta de acceso completa de la subclave que se va a copiar. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<KeyName2 >|Especifica la ruta de acceso completa del destino de la subclave. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para copiar todas las subclaves y valores de la clave MyApp en la clave SaveMyApp, escriba:
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Para copiar todos los valores de la clave MyCo del equipo denominado zodíaco en la clave MyCo1 del equipo actual, escriba:
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)