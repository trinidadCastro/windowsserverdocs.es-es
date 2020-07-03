---
title: expose
description: Artículo de referencia para el comando exponer, que expone una instantánea persistente como una letra de unidad, un recurso compartido o un punto de montaje.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816aad0ba57a30d9d3a05709941b1915d9a97d03
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932406"
---
# <a name="expose"></a>expose

Expone una instantánea persistente como letra de unidad, recurso compartido o punto de montaje.

## <a name="syntax"></a>Sintaxis

```
expose <shadowID> {<drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| shadowID | Especifica el ID. de sombra de la instantánea que desea exponer. También puede usar un alias existente o una variable de entorno en lugar de *shadowID*. Use **Agregar** sin parámetros para ver los alias existentes. |
| `<drive:>` | Expone la instantánea especificada como una letra de unidad (por ejemplo, `p:` ). |
| `<share>` | Expone la instantánea especificada en un recurso compartido (por ejemplo, `\\machinename` ).   |
| `<mountpoint>` | Expone la instantánea especificada en un punto de montaje (por ejemplo, `C:\shadowcopy` ). |

### <a name="examples"></a>Ejemplos

Para exponer la instantánea persistente asociada a la variable de entorno VSS_SHADOW_1 como unidad X, escriba:

```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [DiskShadow (comando)](diskshadow.md)
