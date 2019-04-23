---
title: reg guardar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a46dfe081421ed727bd7ffeeab364e6c23dd801
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841086"
---
# <a name="reg-save"></a>reg guardar



Guarda una copia de las subclaves, entradas y valores del registro en un archivo especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave. Para especificar los equipos remotos, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<FileName>|Especifica el nombre y ruta de acceso del archivo que se crea. Si no se especifica ninguna ruta de acceso, se usa la ruta de acceso actual.|
|/y|Sobrescribe un archivo existente con el nombre *FileName* sin pedir confirmación.|
|/?|Muestra la Ayuda de **reg guardar** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Comentarios \<sección opcional >

-   En la tabla siguiente se enumera los valores devueltos por la **guardar reg** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|
-   Antes de modificar las entradas del registro, guarde la subclave primaria con el **guardar reg** operación. Si se produce un error en la edición, restaure la subclave original con el **reg restauración** operación.

## <a name="BKMK_examples"></a>Ejemplos

Para guardar el subárbol MyApp en la carpeta actual como un archivo denominado CopiaAp.hiv, escriba:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)