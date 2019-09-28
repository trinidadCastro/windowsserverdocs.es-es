---
title: bitsadmin getnoprogresstimeout
description: 'Temas de comandos de Windows para **bitsadmin getnoprogresstimeout** : recupera el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produzca un error transitorio.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dcc0e445f4cae25c27f5ff70c73f4f2f23975aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381502"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout



Recupera el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produzca un error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetNoProgressTimeout <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el valor de tiempo de espera de progreso para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetNoProgressTimeout myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)