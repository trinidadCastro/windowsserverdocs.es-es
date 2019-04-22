---
title: bitsadmin cancel
description: 'Tema de los comandos de Windows para **bitsadmin cancelar** : quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d1e2d6e4fd66cb525316f236d070fcd72d73f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814076"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se quita el *myDownloadJob* trabajo desde la cola de transferencia.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)