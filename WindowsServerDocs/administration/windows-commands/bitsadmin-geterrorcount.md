---
title: bitsadmin geterrorcount
description: Tema de referencia del comando bitsadmin geterrorcount, que recupera un recuento del número de veces que el trabajo especificado generó un error transitorio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 516bd02ed296a2eba75e174c6f084926bde63e90
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718010"
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
