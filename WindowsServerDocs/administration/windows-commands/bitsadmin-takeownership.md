---
title: bitsadmin takeownership
description: Tema de comandos de Windows para bitsadmin TakeOwnerShip, que permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2c0bfc1fcb1606102aece76129c49aad701ead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849028"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /TakeOwnership <Job>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se toma la propiedad del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)