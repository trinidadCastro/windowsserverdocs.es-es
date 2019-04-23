---
title: bitsadmin setdescription
description: Tema de los comandos de Windows para **setdescription bitsadmin** -establece la descripción del trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e3323c20eebc8ba633ccfd478daa0753e506f46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830756"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



Establece la descripción del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Descripción|Texto que se usa para describir el trabajo.|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera la descripción del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)