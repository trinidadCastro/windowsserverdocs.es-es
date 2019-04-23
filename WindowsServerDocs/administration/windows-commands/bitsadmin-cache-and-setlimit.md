---
title: setlimit y bitsadmin caché
description: Tema de los comandos de Windows para **bitsadmin caché y setlimit** -establece el límite de tamaño de caché.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d0b72c5ec6c779fa4ce3fa038352836cd9456ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852596"
---
# <a name="bitsadmin-cache-and-setlimit"></a>setlimit y bitsadmin caché



Establece el límite de tamaño de caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Por ciento|El límite de caché que se define como un porcentaje del espacio total del disco duro...|

## <a name="BKMK_examples"></a>Ejemplos

En el siguiente ejemplo se limita el tamaño de caché en el 50%.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)