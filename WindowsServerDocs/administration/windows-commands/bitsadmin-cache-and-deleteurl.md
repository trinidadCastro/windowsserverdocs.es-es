---
title: memoria caché de bitsadmin y deleteurl
description: Tema de comandos de Windows para la **caché de bitsadmin y deleteurl**, que elimina todas las entradas de la memoria caché de la dirección URL especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70099e795d0f05d0fcf75fbf6b82f5466d1c0c55
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850938"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>memoria caché de bitsadmin y deleteurl

Elimina todas las entradas de la memoria caché de la dirección URL especificada.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /deleteURL url
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| url | Localizador uniforme de recursos que identifica un archivo remoto. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se eliminan todas las entradas de la memoria caché para `https://www.contoso.com/en/us/default.aspx`

```
C:\>bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)