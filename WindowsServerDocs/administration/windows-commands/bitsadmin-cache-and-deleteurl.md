---
title: deleteurl y bitsadmin caché
description: Tema de los comandos de Windows para **bitsadmin caché y deleteurl** -elimina todas las entradas de caché para la dirección URL especificada.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a831c49e1461761cb7466b46e7a5ad8e037f4ec9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816656"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>deleteurl y bitsadmin caché



Elimina todas las entradas de caché para la dirección URL especificada.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|url|El localizador uniforme de recursos que identifica un archivo remoto.|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se eliminan todas las entradas de caché para https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)