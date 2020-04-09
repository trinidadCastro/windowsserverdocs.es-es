---
title: bitsadmin suspend
description: Tema de comandos de Windows para bitsadmin suspender, que suspende el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0419f4cdf59d04539b8b4c6d47cec886197d412b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849058"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Suspende el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Suspend <Job>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Para reiniciar el trabajo, use el conmutador [bitsadmin resume](bitsadmin-resume.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se suspende el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
