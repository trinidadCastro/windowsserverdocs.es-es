---
title: reset
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0701324ad1ee94cc645c7519d81fef7357b6a34a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722355"
---
# <a name="reset"></a>reset



Restablece DiskShadow. exe al estado predeterminado. El **restablecimiento** es especialmente útil para separar las operaciones de DiskShadow compuestas, como **crear**, **importar**, **realizar copias de seguridad**o **restaurar**.

## <a name="syntax"></a>Sintaxis

```
reset
```

## <a name="remarks"></a>Observaciones

-   Cuando se usa el comando de **restablecimiento** , se pierde el estado de los comandos como **Agregar**, **establecer**, **cargar**o **escritor**. **RESET** también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)