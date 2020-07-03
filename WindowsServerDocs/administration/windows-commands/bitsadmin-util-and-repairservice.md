---
title: bitsadmin util y repairservice
description: Artículo de referencia del comando bitsadmin util y repairservice, que corrige problemas conocidos de varias versiones de servicio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf62a9410765914187b6a60ff5376e8ff5aabe03
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927341"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util y repairservice

Si no se inicia BITS, este modificador intenta resolver los errores relacionados con la configuración incorrecta del servicio y las dependencias de los servicios de Windows (por ejemplo, LANManworkstation) y el directorio de red. Este modificador también genera una salida que indica si se resolvieron los problemas.

> [!NOTE]
> Este comando no es compatible con BITS 1,5 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /repairservice [/force]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /force | Opcional. Elimina y crea el servicio de nuevo.|

> [!NOTE]
> Si BITS vuelve a crear el servicio, es posible que la cadena de Descripción del servicio esté establecida en inglés incluso en un sistema localizado.

## <a name="examples"></a>Ejemplos

Para reparar la configuración del servicio BITS:

```
bitsadmin /util /repairservice
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin util (comando)](bitsadmin-util.md)

- [bitsadmin (comando)](bitsadmin.md)
