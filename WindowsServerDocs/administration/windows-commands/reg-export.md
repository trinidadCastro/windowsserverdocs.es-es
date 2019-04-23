---
title: reg export
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7aeddb4b069b1baf5b8f7aaea2730a2b25bdad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889656"
---
# <a name="reg-export"></a>reg export



Copia las subclaves especificadas, entradas y valores del equipo local en un archivo para la transferencia a otros servidores.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave. La operación de exportación solo funciona con el equipo local. El nombre de clave debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<FileName>|Especifica el nombre y ruta de acceso del archivo que se cree durante la operación. El archivo debe tener una extensión. reg.|
|/y|Sobrescribe cualquier archivo existente con el nombre *FileName* sin pedir confirmación.|
|/?|Muestra la Ayuda de **reg export** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumera los valores devueltos por la **reg export** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para exportar el contenido de todas las subclaves y valores de la clave MyApp al archivo CopiaAp.reg, escriba:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)