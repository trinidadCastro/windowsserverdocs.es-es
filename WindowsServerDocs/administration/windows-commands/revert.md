---
title: revertir
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bc77b17317f602d642c7a9e025b67be10ad7256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875116"
---
# <a name="revert"></a>revertir



Revierte un volumen en una copia de instantáneas especificada. Esto solo se admite para las instantáneas en el contexto CLIENTACCESSIBLE. Estas instantáneas son persistentes y solo se pueden realizar por el proveedor del sistema. Si se utiliza sin parámetros, **revertir** muestra la Ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ShadowCopyID>|Especifica el identificador de instantánea para el volumen volverá al.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)