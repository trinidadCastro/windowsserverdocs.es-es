---
title: Comprobación
description: Tema de referencia para Verify, que indica a **cmd** si desea comprobar que los archivos se escriben correctamente en un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7fc35b37d5e0a429e1ecc2ebefc117804a0c645
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720294"
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