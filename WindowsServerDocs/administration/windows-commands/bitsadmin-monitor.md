---
title: bitsadmin monitor
description: Artículo de referencia para el comando bitsadmin monitor, que supervisa los trabajos de la cola de transferencia que son propiedad del usuario actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ce08eccf46fc17086d216bc6797bec451ace7eb
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926494"
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
