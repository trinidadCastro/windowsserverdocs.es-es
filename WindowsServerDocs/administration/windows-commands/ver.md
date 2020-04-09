---
title: ver
description: Comando comandos de Windows para ver, que muestra el número de versión del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d0676dcfa6546e4bbf74c4c58a24f51744d00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830198"
---
# <a name="ver"></a>ver



Muestra el número de versión del sistema operativo.

Este comando se admite en el símbolo del sistema de Windows (cmd. exe), pero no en PowerShell.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ver
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para obtener el número de versión del sistema operativo desde el shell de comandos (cmd. exe), escriba:

```
ver
```

El comando ver no funciona en PowerShell. Para obtener la versión del sistema operativo de PowerShell, escriba:

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
