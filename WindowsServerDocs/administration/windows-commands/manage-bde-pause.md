---
title: la pausa de Manage-BDE
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: efda0e08-b9ff-4e71-83d8-bb666b3032bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2bd5b9fd0eccc491378164efc1f98cb6afb0aa8
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820645"
---
# <a name="manage-bde-pause"></a>Manage-BDE: PAUSE



Pausa el cifrado o descifrado de BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde -pause <Volume> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de volumen|Una letra de unidad seguida de dos puntos, una ruta de acceso de GUID de volumen o un volumen montado.|
|-COMPUTERNAME|Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|\<Name>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Ilustra el uso del comando **-PAUSE** para pausar el cifrado de BitLocker en la unidad C.
```
manage-bde –pause C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)