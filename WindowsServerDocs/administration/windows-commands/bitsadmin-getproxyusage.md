---
title: bitsadmin getproxyusage
description: Tema de los comandos de Windows para **getproxyusage bitsadmin** -recupera la configuración del uso de proxy para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 20ba418b8dfcf3d96d9b20b22e53797a232a13f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863886"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



Recupera la configuración del uso de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Los valores posibles son:
-   PRECONFIGURAR: usar valores predeterminados de Internet Explorer del propietario.
-   NO_PROXY: no use un servidor proxy.
-   REEMPLAZAR: utilizar una lista de proxy explícito.
-   Detección automática: detectar automáticamente la configuración de proxy.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el uso de proxy del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)