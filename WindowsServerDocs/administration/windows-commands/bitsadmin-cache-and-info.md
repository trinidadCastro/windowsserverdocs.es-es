---
title: información y bitsadmin caché
description: Tema de los comandos de Windows para **bitsadmin caché e información** -volcados de memoria de una entrada de caché específica.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ff57b33e575921f2032d4e13a2d9b74accae60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813326"
---
# <a name="bitsadmin-cache-and-info"></a>información y bitsadmin caché



Volcados de memoria de una entrada de caché específica.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|RecordID|El GUID asociado con la entrada de caché.|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente vuelca la entrada de caché con el identificador de registro de {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)