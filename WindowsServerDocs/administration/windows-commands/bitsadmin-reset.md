---
title: bitsadmin reset
description: Tema de los comandos de Windows para **bitsadmin restablece** -cancela todos los trabajos en la cola de transferencia que posee el usuario actual.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7c29aac55393cd87145583814b3ffa8f0a2c3b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874256"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos los trabajos en la cola de transferencia que posee el usuario actual.

**BITSAdmin 1.5 y versiones anterior**: Si tiene privilegios de administrador, **restablecer** cancela todos los trabajos en la cola. No se admite la opción /AllUsers.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|AllUsers|Opcional: cancela todos los trabajos en la cola.|

## <a name="remarks"></a>Comentarios

Debe tener privilegios de administrador para usar el **AllUsers** parámetro.

> [!NOTE]
> Los administradores no pueden restablecer los trabajos creados por el sistema Local. Usar al programador de tareas para programar este comando como una tarea con las credenciales de sistema Local.

## <a name="BKMK_examples"></a>Ejemplos

En el siguiente ejemplo cancela todos los trabajos en la cola de transferencia para el usuario actual.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)