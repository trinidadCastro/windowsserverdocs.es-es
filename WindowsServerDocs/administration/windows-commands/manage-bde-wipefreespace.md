---
title: Manage-BDE WipeFreeSpace
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35d5f5fe079485d35fa412502bec745a136fbce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373778"
---
# <a name="manage-bde-wipefreespace"></a>Manage-BDE WipeFreeSpace



Borra el espacio disponible en el volumen quitando los fragmentos de datos que puedan haber existido en el espacio. Ejecutar este comando en un volumen cifrado mediante el método de cifrado "solo espacio usado" proporciona el mismo nivel de protección que el método de cifrado "cifrado de volumen completo". Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Drive >|Representa una letra de unidad seguida de dos puntos, una ruta de acceso de GUID de volumen o un volumen montado.|
|-Cancelar|Cancela un borrado de espacio disponible que está en curso.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra el uso del comando **-w** para crear el borrado del espacio libre en la unidad C.
```
manage-bde -w C:
```
En el ejemplo siguiente se muestra el uso del comando **-w** con el parámetro **-Cancel** para cancelar la eliminación del espacio libre en la unidad C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)