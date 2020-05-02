---
title: REG Export
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c697595d5d19c953ef85f7a2e334c6fe05329d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722563"
---
# <a name="reg-export"></a>REG Export



Copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores.



## <a name="syntax"></a>Sintaxis

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> KeyName|Especifica la ruta de acceso completa de la subclave. La operación de exportación solo funciona con el equipo local. KeyName debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<Nombre de archivo>|Especifica el nombre y la ruta de acceso del archivo que se va a crear durante la operación. El archivo debe tener una extensión. reg.|
|/y|Sobrescribe cualquier archivo existente con el nombre *filename* sin pedir confirmación.|
|/?|Muestra ayuda para la **exportación reg** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la operación de **exportación del registro** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a>Ejemplos

Para exportar el contenido de todas las subclaves y valores de la clave MyApp al archivo CopiaAp. reg, escriba:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)