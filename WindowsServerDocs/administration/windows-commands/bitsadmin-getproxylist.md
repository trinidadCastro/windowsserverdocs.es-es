---
title: Bitsadmin getproxylist - recupera la lista de proxy para el trabajo especificado.
description: Tema de los comandos de Windows para **getproxylist bitsadmin** -recupera la lista de proxy para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8c3ffb1e425552cda5b14a00287817ace77a90f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840516"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera la lista de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

La lista de proxy es la lista de servidores proxy para usar. La lista está delimitada por comas.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera la lista de proxy del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)