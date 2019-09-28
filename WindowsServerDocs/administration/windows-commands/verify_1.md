---
title: verify
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 840fd3609ed3aded1c9cfebd4e395ddcc6d5588b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363100"
---
# <a name="verify"></a>verify



Indica a **cmd** si desea comprobar que los archivos se escriben correctamente en un disco. Si se usa sin parámetros, **Verify** muestra el valor actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
verify [on | off]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[on \| OFF]|Activa o desactiva la configuración de **comprobación** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Example

Para mostrar la configuración de **verificación** actual, escriba:
```
verify
```
Para activar la configuración de **comprobación** , escriba:
```
Verify on
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)