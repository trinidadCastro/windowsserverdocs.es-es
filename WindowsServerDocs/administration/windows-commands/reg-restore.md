---
title: REG restore
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d490f99f032b38c8bbbe9352b8571b4a85202e1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722521"
---
# <a name="reg-restore"></a>REG restore



Vuelve a escribir las subclaves y entradas guardadas en el registro.



## <a name="syntax"></a>Sintaxis

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> KeyName|Especifica la ruta de acceso completa de la subclave que se va a restaurar. La operación de restauración solo funciona con el equipo local. KeyName debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<Nombre de archivo>|Especifica el nombre y la ruta de acceso del archivo con el contenido que se va a escribir en el registro. Este archivo debe crearse de antemano con la operación de **Guardar reg** mediante la extensión. HIV.|
|/?|Muestra la ayuda de **reg restore** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Antes de editar las entradas del registro, guarde la subclave primaria con la operación **reg Save** . Si se produce un error en la edición, restaure la subclave original con la operación **reg restore** .
-   En la tabla siguiente se enumeran los valores devueltos para la operación de **restauración de reg** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a>Ejemplos

Para restaurar el archivo denominado NTRKBkUp. HIV en la clave HKLM\Software\Microsoft\ResKit y sobrescribir el contenido existente de la clave, escriba:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)