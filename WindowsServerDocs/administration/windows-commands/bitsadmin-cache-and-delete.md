---
title: bitsadmin cache y DELETE
description: Tema de comandos de Windows para la **caché de bitsadmin y DELETE**, que elimina una entrada de caché específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fd7f1db83a62dd9c1085d6afdcf509c1c3ac8cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850948"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache y DELETE

Elimina una entrada de caché específica.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| recordID | GUID asociado a la entrada de caché. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el siguiente ejemplo se elimina la entrada de caché con el RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)