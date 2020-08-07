---
title: ver
description: Artículo de referencia para ver, que muestra el número de versión del sistema operativo.
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de080395c2e26f03371e0b27609238b66d7317f5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891887"
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
