---
title: memoria caché de bitsadmin y setlimit
description: Tema de comandos de Windows para la **caché de bitsadmin y setlimit**, que establece el límite de tamaño de caché.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 746ee0b69da8f5bd22fec2ccbd432126cc25d94d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850878"
---
# <a name="bitsadmin-cache-and-setlimit"></a>memoria caché de bitsadmin y setlimit

Establece el límite de tamaño de la memoria caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| percent | Límite de caché definido como un porcentaje del espacio total del disco duro. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el siguiente ejemplo se limita el tamaño de la caché al 50%.

```
C:\>bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)