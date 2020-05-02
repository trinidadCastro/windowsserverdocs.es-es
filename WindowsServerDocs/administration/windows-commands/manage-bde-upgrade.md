---
title: actualización de Manage-BDE
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23bfa824-6ff0-44cc-9b8b-b199a769fb8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ce7d1805a98a3029ce9b83a1b475ecd8bdb0a4ab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724056"
---
# <a name="manage-bde-upgrade"></a>Manage-BDE: actualización



Actualiza la versión de BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde -upgrade [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad|Representa la letra de una unidad seguida del signo de dos puntos.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Ilustra el uso del comando **-upgrade** para actualizar el cifrado de BitLocker en la unidad C.
```
manage-bde –upgrade C:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)
-   [Actualizar un equipo protegido con BitLocker de Windows Vista a Windows 7](https://technet.microsoft.com/library/ee424325(v=ws.10).aspx)