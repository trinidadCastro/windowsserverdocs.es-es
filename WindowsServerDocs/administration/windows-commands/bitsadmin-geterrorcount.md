---
title: bitsadmin geterrorcount
description: 'Temas de comandos de Windows para **bitsadmin geterrorcount** : recupera un recuento del número de veces que el trabajo especificado generó un error transitorio.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e5aa64c0e080e946e84c0bf804527bb00cad70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381621"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



Recupera un recuento del número de veces que el trabajo especificado generó un error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera información de recuento de errores para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)