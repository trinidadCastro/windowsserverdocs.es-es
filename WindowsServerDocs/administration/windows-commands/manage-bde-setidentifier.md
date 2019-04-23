---
title: ¿Manage-bde setidentifier
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e75483985624e77c5ea454bc3de299c6d0c31035
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831106"
---
# <a name="manage-bde-setidentifier"></a>¿Manage-bde: setidentifier



Establece el campo de identificador de unidad en la unidad para el valor especificado en el **proporcionar identificadores únicos para su organización** configuración de directiva de grupo. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive>|Representa la letra de una unidad seguida del signo de dos puntos.|
|-computername|Especifica que se utilizará Manage-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.|
|-? ¿o /?|Muestra una breve ayuda en el símbolo del sistema.|
|-help o -h|Muestra la Ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **- setidentifier** comando para establecer el campo de identificador de unidad BitLocker para C.
```
manage-bde –setidentifier C:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [¿Manage-bde](manage-bde.md)
-   [Uso de agentes de recuperación de datos con BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)