---
title: bitsadmin setminretrydelay
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 379dfa8bfdc48969f268fd1c9544d3bee8bbe646
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380512"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Establece la cantidad mínima de tiempo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|RetryDelay|Número representado en segundos.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece el intervalo mínimo de reintentos para el trabajo denominado *myDownloadJob* en 35 segundos.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)