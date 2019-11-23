---
title: descarga de reg
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32df397b597291269dcfb1449d00e86b2f4f5836
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384620"
---
# <a name="reg-unload"></a>descarga de reg



Quita una sección del registro que se cargó con la operación de **carga de reg** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg unload <KeyName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave que se va a descargar. Para especificar equipos remotos, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son HKLM y HKU.|
|/?|Muestra ayuda para la **descarga de reg** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la opción **reg Unload** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para descargar el subárbol TempHive en el archivo HKLM, escriba:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> No edite el registro directamente a menos que no tenga ninguna alternativa. El editor del registro omite las medidas de seguridad estándar, lo que permite valores que pueden degradar el rendimiento, dañar el sistema o incluso requerir que se vuelva a instalar Windows. Puede modificar la mayoría de la configuración del registro de forma segura mediante los programas del panel de control o Microsoft Management Console (MMC). Si debe modificar el registro directamente, haga una copia de seguridad en primer lugar.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)