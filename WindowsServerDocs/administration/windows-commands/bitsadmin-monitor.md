---
title: bitsadmin monitor
description: 'Temas de comandos de Windows para el **monitor de bitsadmin** : supervisa los trabajos de la cola de transferencia que posee el usuario actual.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe4963349c7e17fc77500b5adfceafc48a20ac5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381221"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Supervisa los trabajos de la cola de transferencia que posee el usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|AllUsers|Opcional: supervisa los trabajos de todos los usuarios.|
|Actualizar|Opcional: actualiza los datos en un intervalo especificado por *segundos*. El intervalo de actualización predeterminado es de cinco segundos.|

## <a name="remarks"></a>Comentarios

Debe tener privilegios de administrador para usar el parámetro **AllUsers** .

Use CTRL + C para detener la actualización.

## <a name="BKMK_examples"></a>Example

En el siguiente ejemplo se supervisa la cola de transferencia para los trabajos que pertenecen al usuario actual y se actualiza la información cada 60 segundos.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)