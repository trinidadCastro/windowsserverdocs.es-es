---
title: Manage-BDE changekey
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fc273bc84e0bc25a7409941af6dca02b6042640
ms.sourcegitcommit: 479ad84a0d6c7c7b8308122b8bac8308cb36fe9b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391699"
---
# <a name="manage-bde-changekey"></a>Manage-BDE: changekey



Modifica la clave de inicio de una unidad del sistema operativo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<unidad >|Representa la letra de una unidad seguida del signo de dos puntos.|
|\<PathToExternalKeyDirectory >|Representa la ubicación del directorio para guardar el archivo de clave de inicio externo que se puede usar para desbloquear la unidad.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|Nombre del \<>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a><a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra el uso del comando **-changekey** para crear una nueva clave de inicio en la unidad E que se va a usar con el cifrado de BitLocker en la unidad C.
```
manage-bde -changekey C: E:\
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)
