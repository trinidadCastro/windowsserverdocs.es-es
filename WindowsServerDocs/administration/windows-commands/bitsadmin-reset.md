---
title: bitsadmin reset
description: 'Temas de comandos de Windows para **bitsadmin RESET** : cancela todos los trabajos de la cola de transferencia que posee el usuario actual.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: adc6b07a7b5d1414c733fe6a3ac05eba7cb3029e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380810"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos los trabajos de la cola de transferencia que posee el usuario actual.

**BITSAdmin 1,5 y versiones anteriores**: Si tiene privilegios de administrador, **restablezca**@no__t 1cancels todos los trabajos de la cola. No se admite la opción/AllUsers.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|AllUsers|Opcional: cancela todos los trabajos de la cola.|

## <a name="remarks"></a>Comentarios

Debe tener privilegios de administrador para usar el parámetro **AllUsers** .

> [!NOTE]
> Los administradores no pueden restablecer los trabajos creados por el sistema local. Utilice el programador de tareas para programar este comando como una tarea con las credenciales del sistema local.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se cancelan todos los trabajos de la cola de transferencia del usuario actual.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)