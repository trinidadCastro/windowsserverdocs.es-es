---
title: Manage-BDE KeyPackage
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a53edd060cb8c7a7a52e86130b2136893a42150
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840108"
---
# <a name="manage-bde-keypackage"></a>Manage-BDE: KeyPackage



Genera un paquete de claves para una unidad. El paquete de claves se puede usar junto con la herramienta de reparación para reparar unidades dañadas. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<unidad >|Representa la letra de una unidad seguida del signo de dos puntos.|
|-ID|Cree un paquete de claves mediante el protector de clave con el identificador especificado por este valor de identificador.|
|-path|Ubicación en la que se va a guardar el paquete de claves creado.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|Nombre del \<>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En el ejemplo siguiente se muestra cómo usar el comando **-KeyPackage** para crear un paquete de claves para la unidad C según el protector de clave identificado por el GUID y guardar el paquete de claves en F:\Folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Use **Manage-BDE – protectors-Get** junto con la letra de unidad para la que desea crear un paquete de claves para obtener una lista de los GUID disponibles que se usarán como valor de identificador.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)