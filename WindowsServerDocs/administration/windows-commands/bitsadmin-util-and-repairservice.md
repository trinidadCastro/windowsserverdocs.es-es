---
title: bitsadmin util y repairservice
description: Artículo de referencia del comando bitsadmin util y repairservice, que corrige problemas conocidos de varias versiones de servicio BITS.
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d90e6328376f52e60b598d8c2324b59877415db
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880858"
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
