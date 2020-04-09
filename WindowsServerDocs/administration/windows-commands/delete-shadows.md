---
title: eliminar sombras
description: Windows Commands tema para eliminar sombras, que elimina las instantáneas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd109f7ddc0365d03737eddba31a1a4b7f34915b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846568"
---
# <a name="delete-shadows"></a>eliminar sombras

elimina las instantáneas.

## <a name="syntax"></a>Sintaxis

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---- | ---- |
| all | Elimina todas las instantáneas. |
| volumen \<volumen > | Elimina todas las instantáneas del volumen especificado. |
| Volumen de \<más antiguo > | Elimina la instantánea más antigua del volumen especificado. |
| establecer \<> SetId | Elimina las instantáneas del conjunto de instantáneas del ID. especificado. Puede especificar un alias mediante el **%** símbolo si el alias existe en el entorno actual. |
| ID \<ShadowID > | Elimina una instantánea del ID. especificado. Puede especificar un alias mediante el **%** símbolo si el alias existe en el entorno actual. |
| expuesto {\<de la unidad > | <MountPoint>} |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)