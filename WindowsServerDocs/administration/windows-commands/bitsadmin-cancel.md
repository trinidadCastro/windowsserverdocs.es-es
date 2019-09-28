---
title: bitsadmin cancel
description: 'Temas de comandos de Windows para **bitsadmin cancelar** : quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77e46d787359af43a37faba5d844bfec09730454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381797"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se quita el trabajo *myDownloadJob* de la cola de transferencia.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)