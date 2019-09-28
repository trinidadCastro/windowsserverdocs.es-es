---
title: bitsadmin util y repairservice
description: Tema de comandos de Windows para **bitsadmin util y repairservice** -Command usados para corregir problemas conocidos con diversas versiones de servicio bits.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab06ac9c784cfa438eb285c28f0e661cf4b8302
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380277"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util y repairservice

Si no se inicia BITS, use este modificador para corregir problemas conocidos con diversas versiones de BITS.

**BITSAdmin 1,5 y versiones anteriores:** se admite  Not.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Force|Opcional: elimina y vuelve a crear el servicio.|

## <a name="remarks"></a>Comentarios

Este modificador resuelve los errores relacionados con la configuración incorrecta del servicio y las dependencias de los servicios de Windows (por ejemplo, LANManworkstation) y el directorio de red. Este modificador genera un resultado que indica si se resolvieron los problemas.

> [!NOTE]
> Si BITS vuelve a crear el servicio, la cadena de Descripción del servicio se puede establecer en inglés en un sistema localizado.

> [!IMPORTANT]
> Este comando no es compatible con Windows Vista.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se repara la configuración del servicio BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)