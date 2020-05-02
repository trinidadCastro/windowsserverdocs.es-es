---
title: Eliminar registro
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4ff643970bac021a6b7dcb731e64c412deb8df3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722575"
---
# <a name="reg-delete"></a>Eliminar registro



Elimina una subclave o entradas del registro.



## <a name="syntax"></a>Sintaxis

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> KeyName|Especifica la ruta de acceso completa de la subclave o entrada que se va a eliminar. Para especificar un equipo remoto, incluya el nombre del equipo (en el \\ \\formato\) COMPUTERNAME como parte del *keyName*). Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName>|Elimina una entrada específica bajo la subclave. Si no se especifica ninguna entrada, se eliminarán todas las entradas y subclaves de la subclave.|
|/ve|Especifica que solo se eliminarán las entradas que no tengan ningún valor.|
|/va|Elimina todas las entradas de la subclave especificada. Las subclaves de la subclave especificada no se eliminan.|
|/f|Elimina la subclave del registro existente o la entrada sin solicitar confirmación.|
|/?|Muestra la ayuda de **reg Delete** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la operación de **eliminación de registro** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a>Ejemplos

Para eliminar el tiempo de espera de la clave del registro y sus subclaves y valores, escriba:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Para eliminar la MTU del valor del registro en HKLM\Software\MyCo en el equipo denominado zodíaco, escriba:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)