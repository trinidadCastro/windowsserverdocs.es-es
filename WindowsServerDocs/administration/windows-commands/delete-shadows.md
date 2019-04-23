---
title: eliminar las sombras
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b0d5414cb5b60face5245d7ea22d66c8629bfcb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873736"
---
# <a name="delete-shadows"></a>eliminar las sombras



eliminaciones de instantáneas.

## <a name="syntax"></a>Sintaxis

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|all|Elimina todas las instantáneas.|
|volumen \<volumen >|Elimina todas las instantáneas del volumen especificado.|
|más antiguo \<volumen >|Elimina la instantánea más antigua del volumen especificado.|
|establecer \<SetID >|Elimina las instantáneas en el conjunto de copia sombra del identificador especificado. Puede especificar un alias mediante la **%** si existe el alias en el entorno actual de símbolos.|
|Id. de \<IdDeInstantánea >|Elimina una instantánea del identificador especificado. Puede especificar un alias mediante la **%** si existe el alias en el entorno actual de símbolos.|
|expone {\<unidad > | <MountPoint>}|Elimina la instantánea que se exponen en el punto de montaje o letra de unidad especificada. Especificar puntos de montaje como c:\mountPoint o por la letra de unidad como p:.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)