---
title: bitsadmin suspend
description: Tema de los comandos de Windows para **bitsadmin suspender** -suspende el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87e1bbd1b068d68fb60655043735c6c1aeb07707
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825926"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Suspende el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Para reiniciar el trabajo, use el [bitsadmin reanudar](bitsadmin-resume.md) cambie.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente, se suspende el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
