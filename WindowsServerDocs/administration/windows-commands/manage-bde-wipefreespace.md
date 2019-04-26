---
title: manage-bde WipeFreeSpace
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8750094e7357a3aefa307d24abd1470fbf8d2a71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867176"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde: WipeFreeSpace



Elimina el espacio libre en el volumen quitando los fragmentos de datos que puedan haber existido en el espacio. ¿Ejecute este comando en un volumen que se cifró mediante "solo el espacio utilizado? ¿método de cifrado proporciona el mismo nivel de protección como el "cifrado de volumen completo? método de cifrado. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde –WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive>|Representa una letra de unidad seguida de dos puntos, una ruta de acceso GUID de volumen o un volumen montado.|
|: Cancelar|Cancela una eliminación de espacio libre que se encuentra en proceso.|
|-computername|Especifica que se utilizará Manage-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.|
|-? ¿o /?|Muestra una breve ayuda en el símbolo del sistema.|
|-help o -h|Muestra la Ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **-w** borrado de comando para crear el espacio libre en la unidad C.
```
manage-bde -w C:
```
El ejemplo siguiente se muestra cómo utilizar el **-w** comando con el **– cancelar** parámetro para cancelar el borrado del espacio libre en la unidad C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [¿Manage-bde](manage-bde.md)