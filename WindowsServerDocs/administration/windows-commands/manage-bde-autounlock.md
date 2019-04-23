---
title: desbloqueo automático ¿Manage-bde
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c17f9781dd4ff924358de490162c6388312ce03
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832276"
---
# <a name="manage-bde-autounlock"></a>manage-bde: autounlock



Administra el desbloqueo automático de unidades de datos protegidas por BitLocker. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-habilitar|Habilita el desbloqueo automático de una unidad de datos.|
|-deshabilitar|Deshabilita el desbloqueo automático de una unidad de datos.|
|-clearallkeys|Quita todo ello almacenadas claves externas en la unidad del sistema operativo.|
|\<Drive>|Representa la letra de una unidad seguida del signo de dos puntos.|
|-computername|Especifica que se utilizará Manage-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.|
|-? ¿o /?|Muestra una breve ayuda en el símbolo del sistema.|
|-help o -h|Muestra la Ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **- desbloqueo automático** comando para habilitar el desbloqueo automático de la unidad de datos E.
```
manage-bde –autounlock -enable E:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [¿Manage-bde](manage-bde.md)