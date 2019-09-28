---
title: eliminar sombras
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c965af8b045c5ab3a110542d148b255f382a95c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378630"
---
# <a name="delete-shadows"></a>eliminar sombras



Elimina las instantáneas.

## <a name="syntax"></a>Sintaxis

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                             Descripción                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        all        |                                                                      Elimina todas las instantáneas.                                                                      |
| Volume \<Volume >  |                                                            Elimina todas las instantáneas del volumen especificado.                                                            |
| @no__t más antigua >  |                                                         Elimina la instantánea más antigua del volumen especificado.                                                          |
|   establecer @no__t 0SetID >    | Elimina las instantáneas del conjunto de instantáneas del ID. especificado. Puede especificar un alias mediante el símbolo **%** si el alias existe en el entorno actual. |
|  ID \<ShadowID >   |              Elimina una instantánea del ID. especificado. Puede especificar un alias mediante el símbolo **%** si el alias existe en el entorno actual.               |
| expuesto {\<Drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)