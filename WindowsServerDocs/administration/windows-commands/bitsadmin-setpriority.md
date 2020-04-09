---
title: bitsadmin setpriority
description: Tema de comandos de Windows para bitsadmin SetPriority, que establece la prioridad del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d007c62402a3d70910e1c79fab5c406295a63a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849218"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Establece la prioridad del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetPriority <Job> <Priority>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Priority|Uno de los siguientes valores:</br>-FOREGROUND</br>-ALTO</br>-NORMAL</br>-LOW|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece la prioridad del trabajo denominado *myDownloadJob* en normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)