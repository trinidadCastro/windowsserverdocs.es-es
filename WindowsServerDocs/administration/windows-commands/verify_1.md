---
title: Comprobación
description: Artículo de referencia para Verify, que indica a **cmd** si desea comprobar que los archivos se escriben correctamente en un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1455705d409e0273e85135a183279835e7238d7a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931313"
---
# <a name="verify"></a>Comprobación



Indica a **cmd** si desea comprobar que los archivos se escriben correctamente en un disco. Si se usa sin parámetros, **Verify** muestra el valor actual.



## <a name="syntax"></a>Sintaxis

```
verify [on | off]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[on \| OFF]|Activa o desactiva la configuración de **comprobación** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para mostrar la configuración de **verificación** actual, escriba:
```
verify
```
Para activar la configuración de **comprobación** , escriba:
```
Verify on
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)