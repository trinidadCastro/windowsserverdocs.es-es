---
title: break
description: 'Windows Commands topic for **break_1** : establece o borra la comprobación extendida de Ctrl + C en sistemas ms-dos. Si se usa sin parámetros, **break** muestra la configuración actual. '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73afdac29efbfd9efec88d297cf4185ca1b92d62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380490"
---
# <a name="break"></a>break



Establece o borra la comprobación extendida de CTRL + C en sistemas MS-DOS. Si se usa sin parámetros, **break** muestra la configuración actual.

> [!NOTE]
> Este comando ya no está en uso. Sólo está incluido para conservar la compatibilidad con archivos de MS-DOS existentes, pero no tiene ningún efecto en la línea de comandos debido a que la funcionalidad es automática.

## <a name="syntax"></a>Sintaxis

```
break=[on|off]
```

## <a name="remarks"></a>Comentarios

Si las extensiones de comando se habilitan y se ejecutan en la plataforma Windows, la inserción del comando **break** en un archivo por lotes entra en un punto de interrupción codificado de forma rígida si lo depura un depurador.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)