---
title: REG Import
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7e033091752f97086fd27fcb94e62469f0cced
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722552"
---
# <a name="reg-import"></a>REG Import



Copia el contenido de un archivo que contiene las subclaves del registro exportadas, las entradas y los valores en el registro del equipo local.



## <a name="syntax"></a>Sintaxis

```
Reg import FileName
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Nombre de archivo>|Especifica el nombre y la ruta de acceso del archivo que tiene el contenido que se va a copiar en el registro del equipo local. Este archivo debe crearse de antemano mediante **reg Export**.|
|/?|Muestra ayuda para **reg Import** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la operación de **importación de reg** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a>Ejemplos

Para importar las entradas del registro desde el archivo llamado CopiaAp. reg, escriba:
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)