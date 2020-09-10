---
title: ver
description: Artículo de referencia para ver, que muestra el número de versión del sistema operativo.
ms.topic: reference
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1dcbfc9857d23759f919ad01d98b0436b673206e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636332"
---
# <a name="ver"></a>ver



Muestra el número de versión del sistema operativo.

Este comando se admite en el símbolo del sistema de Windows (Cmd.exe), pero no en PowerShell.



## <a name="syntax"></a>Sintaxis

```
ver
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para obtener el número de versión del sistema operativo desde el shell de comandos (cmd.exe), escriba:

```
ver
```

El comando ver no funciona en PowerShell. Para obtener la versión del sistema operativo de PowerShell, escriba:

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
