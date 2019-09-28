---
title: bitsadmin setmaxdownloadtime
description: 'Temas de comandos de Windows para **bitsadmin setmaxdownloadtime** : establece el tiempo de espera de descarga en segundos.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985453de5bd2f4a06b5635ae5b0a9794d30175b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380560"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime



Establece el tiempo de espera de descarga en segundos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|tiempoDeEspera|Tiempo de espera en segundos|

## <a name="remarks"></a>Comentarios

-   N/D

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece el tiempo de espera para el trabajo denominado *myDownloadJob* en 10 segundos.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)