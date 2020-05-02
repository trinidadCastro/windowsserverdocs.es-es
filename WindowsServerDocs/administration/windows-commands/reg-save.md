---
title: Guardar registro
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd3e932e67df7eb972bd625ecec24f986cf3f3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722512"
---
# <a name="reg-save"></a>Guardar registro



Guarda una copia de las subclaves, entradas y valores especificados del registro en un archivo especificado.



## <a name="syntax"></a>Sintaxis

```
reg save <KeyName> <FileName> [/y]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> KeyName|Especifica la ruta de acceso completa de la subclave. Para especificar equipos remotos, incluya el nombre del equipo (en el \\ \\formato\) COMPUTERNAME como parte del *keyName*). Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<Nombre de archivo>|Especifica el nombre y la ruta de acceso del archivo que se crea. Si no se especifica ninguna ruta de acceso, se usa la ruta de acceso actual.|
|/y|Sobrescribe un archivo existente con el nombre *filename* sin pedir confirmación.|
|/?|Muestra la ayuda de **reg Save** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Comentario \<sección opcional>

-   En la tabla siguiente se enumeran los valores devueltos para la operación de **guardado del registro** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|
-   Antes de editar las entradas del registro, guarde la subclave primaria con la operación **reg Save** . Si se produce un error en la edición, restaure la subclave original con la operación **reg restore** .

## <a name="examples"></a>Ejemplos

Para guardar el MyApp de Hive en la carpeta actual como un archivo denominado CopiaAp. HIV, escriba:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)