---
title: bitsadmin nowrap
description: Tema de los comandos de Windows para **bitsadmin nowrap** -trunca cualquier línea de texto de salida extender más allá del borde más a la derecha de la ventana de comandos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4130f606a6b1874e1ea31952160de44d6e09c6b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822926"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca cualquier línea de texto de salida extender más allá del borde más a la derecha de la ventana de comandos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Comentarios

De forma predeterminada, todos los modificadores, excepto el **Monitor** modificador, ajustar el resultado. Especifique el **NoWrap** cambiar antes de otros modificadores.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob* y no se ajusta a la salida
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)