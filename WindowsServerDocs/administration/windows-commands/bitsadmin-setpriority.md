---
title: bitsadmin setpriority
description: Tema de los comandos de Windows para **bitsadmin setpriority** -establece la prioridad del trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 072f22ae8c928d427104062b8cbf0f8f42ac4416
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882216"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Establece la prioridad del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Prioridad|Uno de los siguientes valores:</br>: PRIMER PLANO</br>-ALTA</br>: NORMAL</br>-LOW|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se establece la prioridad del trabajo denominado *myDownloadJob* a la normalidad.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)