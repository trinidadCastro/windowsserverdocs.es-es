---
title: bitsadmin setdisplayname
description: Windows Commands topic for bitsadmin setDisplayName, que establece el nombre para mostrar del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 601c5b406132e70fb7d4facb97329f7456002bb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849548"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Establece el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|DisplayName|Texto que se usa para el nombre para mostrar del trabajo especificado.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece el nombre para mostrar del trabajo denominado *myDownloadJob* en *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob Download Music Job
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)