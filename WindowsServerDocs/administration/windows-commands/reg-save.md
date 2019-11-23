---
title: Guardar registro
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ae07cd3c90c51e7bd494bc6c35919680cde912a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371703"
---
# <a name="reg-save"></a>Guardar registro



Guarda una copia de las subclaves, entradas y valores especificados del registro en un archivo especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave. Para especificar equipos remotos, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<nombre de archivo >|Especifica el nombre y la ruta de acceso del archivo que se crea. Si no se especifica ninguna ruta de acceso, se usa la ruta de acceso actual.|
|/y|Sobrescribe un archivo existente con el nombre *filename* sin pedir confirmación.|
|/?|Muestra la ayuda de **reg Save** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Comentarios \<sección opcional >

-   En la tabla siguiente se enumeran los valores devueltos para la operación de **guardado del registro** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|
-   Antes de editar las entradas del registro, guarde la subclave primaria con la operación **reg Save** . Si se produce un error en la edición, restaure la subclave original con la operación **reg restore** .

## <a name="BKMK_examples"></a>Example

Para guardar el MyApp de Hive en la carpeta actual como un archivo denominado CopiaAp. HIV, escriba:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)