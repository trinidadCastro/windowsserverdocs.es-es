---
title: reset
description: Artículo de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a412eff7bdf432608a999edb4531074ed5e8f26
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933090"
---
# <a name="reset"></a>reset



Restablece DiskShadow.exe al estado predeterminado. El **restablecimiento** es especialmente útil para separar las operaciones de DiskShadow compuestas, como **crear**, **importar**, **realizar copias de seguridad**o **restaurar**.

## <a name="syntax"></a>Sintaxis

```
reset
```

## <a name="remarks"></a>Comentarios

-   Cuando se usa el comando de **restablecimiento** , se pierde el estado de los comandos como **Agregar**, **establecer**, **cargar**o **escritor**. **RESET** también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)