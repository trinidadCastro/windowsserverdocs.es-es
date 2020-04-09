---
title: Manage-BDE WipeFreeSpace
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a9995c6872380af61bec5d3b200e733c034ea6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839718"
---
# <a name="manage-bde-wipefreespace"></a>Manage-BDE: WipeFreeSpace



Borra el espacio disponible en el volumen quitando los fragmentos de datos que puedan haber existido en el espacio. Ejecutar este comando en un volumen cifrado mediante el método de cifrado solo del espacio usado proporciona el mismo nivel de protección que el método de cifrado de cifrado de volumen completo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<unidad >|Representa una letra de unidad seguida de dos puntos, una ruta de acceso de GUID de volumen o un volumen montado.|
|-Cancelar|Cancela un borrado de espacio disponible que está en curso.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|Nombre del \<>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En el ejemplo siguiente se muestra el uso del comando **-w** para crear el borrado del espacio libre en la unidad C.
```
manage-bde -w C:
```
En el ejemplo siguiente se muestra el uso del comando **-w** con el parámetro **-Cancel** para cancelar la eliminación del espacio libre en la unidad C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)