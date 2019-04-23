---
title: ver
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 384a5e8adb6c8304033f7dc645184ff2b674ae39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887176"
---
# <a name="ver"></a>ver



Muestra el número de versión del sistema operativo.

Este comando se admite en la línea de comandos de Windows (Cmd.exe), pero no en PowerShell.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ver
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Ejemplos

Para obtener el número de versión del sistema operativo desde el shell de comandos (cmd.exe), escriba:

```
ver
```

El comando ver no funciona en PowerShell. Para obtener la versión del SO desde PowerShell, escriba:

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
