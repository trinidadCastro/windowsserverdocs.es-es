---
title: reset
description: Artículo de referencia de * * * *-
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f491a3d8c805ac6b3558f3778f6eba91a7551b82
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883658"
---
# <a name="reset"></a>reset



Restablece DiskShadow.exe al estado predeterminado. El **restablecimiento** es especialmente útil para separar las operaciones de DiskShadow compuestas, como **crear**, **importar**, **realizar copias de seguridad**o **restaurar**.

## <a name="syntax"></a>Sintaxis

```
reset
```

## <a name="remarks"></a>Observaciones

-   Cuando se usa el comando de **restablecimiento** , se pierde el estado de los comandos como **Agregar**, **establecer**, **cargar**o **escritor**. **RESET** también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)