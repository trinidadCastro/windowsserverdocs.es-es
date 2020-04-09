---
title: bitsadmin util y repairservice
description: Tema de comandos de Windows para bitsadmin util y repairservice, que corrige problemas conocidos de varias versiones de servicio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaaa6edab22031dc53d266984bb669634e3bb362
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848898"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util y repairservice

Si BITS no se inicia, use este modificador para corregir problemas conocidos en varias versiones de BITS.

**BITSAdmin 1,5 y versiones anteriores:**  no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /RepairService [/Force]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Force|Opcional: elimina y vuelve a crear el servicio.|

## <a name="remarks"></a>Comentarios

Este modificador resuelve los errores relacionados con la configuración incorrecta del servicio y las dependencias de los servicios de Windows (por ejemplo, LANManworkstation) y el directorio de red. Este modificador genera un resultado que indica si se resolvieron los problemas.

> [!NOTE]
> Si BITS vuelve a crear el servicio, la cadena de Descripción del servicio se puede establecer en inglés en un sistema localizado.

> [!IMPORTANT]
> Este comando no es compatible con Windows Vista.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se repara la configuración del servicio BITS.
```
C:\>bitsadmin /Util /RepairService
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)