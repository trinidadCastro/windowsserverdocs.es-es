---
title: bitsadmin geterrorcount
description: Tema de comandos de Windows para **bitsadmin geterrorcount**, que recupera un recuento del número de veces que el trabajo especificado generó un error transitorio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef0bf043517d4edfa8d72888746ca5d9c92ecc21
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123139"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera información de recuento de errores para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)