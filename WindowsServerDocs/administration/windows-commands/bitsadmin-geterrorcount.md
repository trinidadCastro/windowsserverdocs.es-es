---
title: bitsadmin geterrorcount
description: Artículo de referencia para el comando bitsadmin geterrorcount, que recupera un recuento del número de veces que el trabajo especificado generó un error transitorio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90eaa150f2decba4bbee693ac117cd269d5a7c97
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923054"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

Recupera un recuento del número de veces que el trabajo especificado generó un error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar información de recuento de errores para el trabajo denominado *myDownloadJob*:

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
