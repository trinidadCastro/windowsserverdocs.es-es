---
title: Manage-BDE changekey
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b5f152e4e98387adb7e7780f8458edd409b1a9d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724201"
---
# <a name="manage-bde-changekey"></a>Manage-BDE: changekey



Modifica la clave de inicio de una unidad del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad|Representa la letra de una unidad seguida del signo de dos puntos.|
|\<> PathToExternalKeyDirectory|Representa la ubicación del directorio para guardar el archivo de clave de inicio externo que se puede usar para desbloquear la unidad.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Ilustra el uso del comando **-changekey** para crear una nueva clave de inicio en la unidad E para usarla con el cifrado de BitLocker en la unidad C.
```
manage-bde -changekey C: E:\
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)
