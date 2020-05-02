---
title: bitsadmin getmaxdownloadtime
description: Tema de referencia del comando bitsadmin getmaxdownloadtime, que recupera el tiempo de espera de descarga en segundos.
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c63acee7629267ed10df11fb8cf4eeb0c911e118
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717867"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera el tiempo de espera de descarga en segundos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para obtener el tiempo de descarga máximo para el trabajo llamado *myDownloadJob* en segundos:

```
bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
