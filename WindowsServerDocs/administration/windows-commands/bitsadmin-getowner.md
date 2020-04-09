---
title: bitsadmin getowner
description: Tema de comandos de Windows para bitsadmin **GetOwner**, que recupera el propietario del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e622c3759c9ec20867c693539c4481c70aa4f26
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850568"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Muestra el nombre para mostrar o el GUID del propietario del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se muestra el propietario del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)