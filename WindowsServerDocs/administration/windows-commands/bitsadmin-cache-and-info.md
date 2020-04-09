---
title: memoria caché y información de bitsadmin
description: Tema de comandos de Windows para la **memoria caché de bitsadmin y la información**, que vuelca una entrada de caché específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e9c6ce1eb972a76408483b8a27a3abca5500e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850898"
---
# <a name="bitsadmin-cache-and-info"></a>memoria caché y información de bitsadmin

Vuelca una entrada específica de la memoria caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Parámetros

| Paramreter | Descripción |
| -------------- | -------------- |
| recordID | GUID asociado a la entrada de caché. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se vuelca la entrada de caché con el valor recordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)