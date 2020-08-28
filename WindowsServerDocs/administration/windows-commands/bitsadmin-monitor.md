---
title: bitsadmin monitor
description: Artículo de referencia para el comando bitsadmin monitor, que supervisa los trabajos de la cola de transferencia que son propiedad del usuario actual.
ms.topic: reference
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4188301d1f76a84762841982f782575bcfb912b1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026673"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

Supervisa los trabajos de la cola de transferencia que son propiedad del usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| /allusers | Opcional. Supervisa los trabajos de todos los usuarios. Debe tener privilegios de administrador para usar este parámetro. |
| demorará | Opcional. Actualiza los datos en un intervalo especificado por `<seconds>` . El intervalo de actualización predeterminado es de cinco segundos. Para detener la actualización, presione CTRL + C. |

## <a name="examples"></a>Ejemplos

Para supervisar la cola de transferencia para los trabajos que pertenecen al usuario actual y actualiza la información cada 60 segundos.

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
