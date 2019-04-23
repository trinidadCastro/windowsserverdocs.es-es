---
title: restauración del registro
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0025a37ed8ca50b47e7750501a7362659b500537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858776"
---
# <a name="reg-restore"></a>restauración del registro



Escribe guardado subclaves y entradas de realizar una copia en el registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave que se va a restaurar. La operación de restauración sólo funciona con el equipo local. El nombre de clave debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<FileName>|Especifica el nombre y ruta de acceso del archivo con el contenido se escriban en el registro. Este archivo debe crearse de antemano con el **guardar reg** operación mediante una extensión hiv.|
|/?|Muestra la Ayuda de **reg restauración** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Antes de modificar las entradas del registro, guarde la subclave primaria con el **guardar reg** operación. Si se produce un error en la edición, restaure la subclave original con el **reg restauración** operación.
-   En la tabla siguiente se enumera los valores devueltos por la **reg restauración** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para restaurar el archivo denominado NTRKBkUp.hiv en la clave HKLM\Software\Microsoft\ResKit y sobrescribir el contenido existente de la clave, escriba:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)