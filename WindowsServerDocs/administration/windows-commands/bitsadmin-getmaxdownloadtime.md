---
title: bitsadmin getmaxdownloadtime
description: Tema de comandos de Windows para **bitsadmin getmaxdownloadtime**, que recupera el tiempo de espera de descarga en segundos.
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b6e4a45da76d5ba39edae151454ad7f28a74085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850638"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera el tiempo de espera de descarga en segundos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se obtiene el tiempo de descarga máximo para el trabajo denominado *myDownloadJob* en segundos.

```
C:\>bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
