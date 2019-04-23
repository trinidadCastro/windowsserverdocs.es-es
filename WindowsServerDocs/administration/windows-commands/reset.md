---
title: Restablecer
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0bd9b6735697cbcefdcf68dc3d4a53a6870a7a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850966"
---
# <a name="reset"></a>Restablecer



DiskShadow.exe se restablece al estado predeterminado. **Restablecer** es especialmente útil en la separación, como las operaciones compuestas de DiskShadow **crear**, **importar**, **copia de seguridad**, o **restaurar**.

## <a name="syntax"></a>Sintaxis

```
reset
```

## <a name="remarks"></a>Comentarios

-   Cuando se usa el **restablecer** comando, se pierde el estado de comandos como **agregar**, **establecer**, **cargar**, o **escritor**. **Restablecer** también libera las interfaces IVssBackupComponent y pierde instantáneas no persistentes.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)