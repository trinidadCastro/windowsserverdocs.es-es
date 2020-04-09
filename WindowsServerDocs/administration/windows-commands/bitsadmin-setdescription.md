---
title: bitsadmin setdescription
description: Windows Commands topic for bitsadmin setDescription, que establece la descripción del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a17f864e3bc3b3cdc8ba0d76d553bcfcef27d29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849568"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Establece la descripción del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetDescription <Job> <Description>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Descripción|Texto que se usa para describir el trabajo.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la descripción del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob Music Downloads
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)