---
title: REPAIRSERVICE y bitsadmin util
description: Tema de los comandos de Windows para **bitsadmin util y repairservice** -comando utilizado para solucionar los problemas conocidos con distintas versiones del servicio de BITS.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc5101378a389c865f5753146b711be0d15c6785
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852096"
---
# <a name="bitsadmin-util-and-repairservice"></a>REPAIRSERVICE y bitsadmin util

Si no se puede iniciar BITS, utilice este modificador para solucionar los problemas conocidos con distintas versiones de BITS.

**BITSAdmin 1.5 y versiones anterior:** no compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Force|Opcional: elimina y vuelve a crear el servicio.|

## <a name="remarks"></a>Comentarios

Este modificador se resuelve como la configuración de servicio relacionados con errores y las dependencias de servicios de Windows (por ejemplo, LANManworkstation) y el directorio de red. Este modificador genera un resultado que indica si los problemas que se han resuelto.

> [!NOTE]
> Si BITS vuelve a crear el servicio, se puede establecer la cadena de descripción de servicio para inglés en un sistema localizado.

> [!IMPORTANT]
> Este comando no se admite en Windows Vista.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente repara la configuración del servicio de BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)