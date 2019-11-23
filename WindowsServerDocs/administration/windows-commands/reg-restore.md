---
title: REG restore
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6c121256cecaebc26e2c402d9b9ced8890eddc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384675"
---
# <a name="reg-restore"></a>REG restore



Vuelve a escribir las subclaves y entradas guardadas en el registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave que se va a restaurar. La operación de restauración solo funciona con el equipo local. KeyName debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<nombre de archivo >|Especifica el nombre y la ruta de acceso del archivo con el contenido que se va a escribir en el registro. Este archivo debe crearse de antemano con la operación de **Guardar reg** mediante la extensión. HIV.|
|/?|Muestra la ayuda de **reg restore** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Antes de editar las entradas del registro, guarde la subclave primaria con la operación **reg Save** . Si se produce un error en la edición, restaure la subclave original con la operación **reg restore** .
-   En la tabla siguiente se enumeran los valores devueltos para la operación de **restauración de reg** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para restaurar el archivo denominado NTRKBkUp. HIV en la clave HKLM\Software\Microsoft\ResKit y sobrescribir el contenido existente de la clave, escriba:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)