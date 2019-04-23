---
title: reg unload
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaa7d7a9fa82db2968d988e3b7b3fb8275a72337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834986"
---
# <a name="reg-unload"></a>reg unload



Quita una sección del registro que se cargaron utilizando la **reg carga** operación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg unload <KeyName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave que se descargue. Para especificar los equipos remotos, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Las claves raíz válido para el equipo local son HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son HKLM y HKU.|
|/?|Muestra la Ayuda de **reg unload** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumera los valores devueltos por la **reg unload** opción.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para descargar el subárbol Subarbtemp en el archivo HKLM, escriba:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> No modifique el registro directamente a menos que no haya ninguna alternativa. El editor del registro omite la seguridad estándar, lo que permite la configuración que puede degradar el rendimiento, dañar el sistema o incluso obligar a reinstalar Windows. Mayoría de las configuraciones del registro se puede modificar de forma segura mediante el uso de los programas en el Panel de Control o Microsoft Management Console (MMC). Si se debe modificar el registro directamente, copia de seguridad.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)