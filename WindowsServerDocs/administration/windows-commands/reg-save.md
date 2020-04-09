---
title: Guardar registro
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b1f7829aedc42c0b75bda951572a4c944798ec6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836358"
---
# <a name="reg-save"></a>Guardar registro



Guarda una copia de las subclaves, entradas y valores especificados del registro en un archivo especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg save <KeyName> <FileName> [/y]
```

### <a name="parameters"></a>Parámetros

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

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para guardar el MyApp de Hive en la carpeta actual como un archivo denominado CopiaAp. HIV, escriba:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)