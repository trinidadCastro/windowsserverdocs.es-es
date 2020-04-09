---
title: REG Export
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5901014511fed0c17a641e1ed183ddbf40dd44c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836498"
---
# <a name="reg-export"></a>REG Export



Copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave. La operación de exportación solo funciona con el equipo local. KeyName debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<nombre de archivo >|Especifica el nombre y la ruta de acceso del archivo que se va a crear durante la operación. El archivo debe tener una extensión. reg.|
|/y|Sobrescribe cualquier archivo existente con el nombre *filename* sin pedir confirmación.|
|/?|Muestra ayuda para la **exportación reg** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumeran los valores devueltos para la operación de **exportación del registro** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para exportar el contenido de todas las subclaves y valores de la clave MyApp al archivo CopiaAp. reg, escriba:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)