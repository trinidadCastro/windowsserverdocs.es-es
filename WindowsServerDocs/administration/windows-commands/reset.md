---
title: reset
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c27ddd93d06670a30f797bd58dd396a9e7ce70a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835788"
---
# <a name="reset"></a>reset



Restablece DiskShadow. exe al estado predeterminado. El **restablecimiento** es especialmente útil para separar las operaciones de DiskShadow compuestas, como **crear**, **importar**, **realizar copias de seguridad**o **restaurar**.

## <a name="syntax"></a>Sintaxis

```
reset
```

## <a name="remarks"></a>Comentarios

-   Cuando se usa el comando de **restablecimiento** , se pierde el estado de los comandos como **Agregar**, **establecer**, **cargar**o **escritor**. **RESET** también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)