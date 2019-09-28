---
title: Determinado
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8903c300d12a019f8fb4aef6d367131a195d034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371462"
---
# <a name="reset"></a>Determinado



Restablece DiskShadow. exe al estado predeterminado. El **restablecimiento** es especialmente útil para separar las operaciones de DiskShadow compuestas, como **crear**, **importar**, **realizar copias de seguridad**o **restaurar**.

## <a name="syntax"></a>Sintaxis

```
reset
```

## <a name="remarks"></a>Comentarios

-   Cuando se usa el comando de **restablecimiento** , se pierde el estado de los comandos como **Agregar**, **establecer**, **cargar**o **escritor**. **RESET** también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)