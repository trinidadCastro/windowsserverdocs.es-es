---
title: descarga de reg
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5fd1436ed1122a09eea11d358a3711aedddf2c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836278"
---
# <a name="reg-unload"></a>descarga de reg



Quita una sección del registro que se cargó con la operación de **carga de reg** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg unload <KeyName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave que se va a descargar. Para especificar equipos remotos, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son HKLM y HKU.|
|/?|Muestra ayuda para la **descarga de reg** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumeran los valores devueltos para la opción **reg Unload** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para descargar el subárbol TempHive en el archivo HKLM, escriba:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> No edite el registro directamente a menos que no tenga ninguna alternativa. El editor del registro omite las medidas de seguridad estándar, lo que permite valores que pueden degradar el rendimiento, dañar el sistema o incluso requerir que se vuelva a instalar Windows. Puede modificar la mayoría de la configuración del registro de forma segura mediante los programas del panel de control o Microsoft Management Console (MMC). Si debe modificar el registro directamente, haga una copia de seguridad en primer lugar.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)