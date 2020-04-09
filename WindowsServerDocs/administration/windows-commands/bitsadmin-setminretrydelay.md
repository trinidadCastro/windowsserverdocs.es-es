---
title: bitsadmin setminretrydelay
description: Windows Commands topic for bitsadmin setminretrydelay, que establece el tiempo mínimo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb2fe4c6d0e4f90c6ec49fa1da63404393d4f634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849368"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Establece la cantidad mínima de tiempo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|RetryDelay|Número representado en segundos.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece el intervalo mínimo de reintentos para el trabajo denominado *myDownloadJob* en 35 segundos.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)