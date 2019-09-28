---
title: bitsadmin getproxyusage
description: 'Temas de comandos de Windows para **bitsadmin getproxyusage** : recupera la configuración de uso del proxy para el trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea9a22f4fb35af3436d02d9f23b62ce0888a26b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381292"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



Recupera la configuración de uso del proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Los valores posibles son:
-   Preconfiguración: Use los valores predeterminados de Internet Explorer del propietario.
-   NO_PROXY: no se usa un servidor proxy.
-   Invalidar: usa una lista de proxy explícita.
-   DETECCIÓN automática: detecta automáticamente la configuración de proxy.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el uso de proxy para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)