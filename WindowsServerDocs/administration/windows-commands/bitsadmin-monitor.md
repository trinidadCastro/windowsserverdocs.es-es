---
title: bitsadmin monitor
description: Tema de los comandos de Windows para **bitsadmin monitor** -supervisa trabajos en la cola de transferencia que posee el usuario actual.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c4620d5c8e46cb8bfcb6b9c83261d57781abea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814596"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Supervisa los trabajos en la cola de transferencia que posee el usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ALLUSERS|Opcional: supervisa trabajos para todos los usuarios.|
|Actualizar|Opcional: actualiza los datos en un intervalo especificado por *segundos*. El intervalo de actualización predeterminado es de cinco segundos.|

## <a name="remarks"></a>Comentarios

Debe tener privilegios de administrador para usar el **Allusers** parámetro.

Use CTRL + C para detener la actualización.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente supervisa la cola de transferencia para los trabajos que pertenecen al usuario actual y actualiza la información de cada 60 segundos.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)