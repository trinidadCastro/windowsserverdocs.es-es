---
title: ver
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e48b3b1061edf793c88693b3353753c6a4cedcfc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362721"
---
# <a name="ver"></a>ver



Muestra el número de versión del sistema operativo.

Este comando se admite en el símbolo del sistema de Windows (cmd. exe), pero no en PowerShell.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ver
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Example

Para obtener el número de versión del sistema operativo desde el shell de comandos (cmd. exe), escriba:

```
ver
```

El comando ver no funciona en PowerShell. Para obtener la versión del sistema operativo de PowerShell, escriba:

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
