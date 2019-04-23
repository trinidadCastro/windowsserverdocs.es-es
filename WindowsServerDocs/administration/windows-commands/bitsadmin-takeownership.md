---
title: bitsadmin takeownership
description: Tema de los comandos de Windows para **bitsadmin takeownership** -permite a un usuario con privilegios de administrador tomar posesión del trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aedca49e43588ab51f84477cf8690cf58486c3cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827846"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership



Permite a los usuarios con privilegios de administrador tomar posesión del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /TakeOwnership <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente toma posesión del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)