---
title: bitsadmin getclientcertificate
description: En el tema comandos de Windows para **bitsadmin getclientcertificate** se recupera el certificado de cliente del trabajo.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 613feafb442f63513d34e9038647c4dbeb278630
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381722"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate



Recupera el certificado de cliente del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el certificado de cliente para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)