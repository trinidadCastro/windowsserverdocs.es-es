---
title: bitsadmin complete
description: 'Comando comandos de Windows para **bitsadmin completado** : completa el trabajo. Los archivos descargados no estarán disponibles hasta que use este modificador.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5a1dc5dbbf2d5b3207b5423f338e0caf4412599
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381820"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa el trabajo. Los archivos descargados no estarán disponibles hasta que use este modificador. Use este modificador después de que el trabajo se mueva al estado transferido. De lo contrario, solo estarán disponibles los archivos que se hayan transferido correctamente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

Cuando se transfiere el estado del trabajo, BITS ha transferido correctamente todos los archivos del trabajo. Sin embargo, los archivos no estarán disponibles hasta que use el modificador **/Complete** Si varios trabajos usan *myDownloadJob* como su nombre, debe reemplazar *MYDOWNLOADJOB* por el GUID del trabajo para identificar el trabajo de forma única.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)