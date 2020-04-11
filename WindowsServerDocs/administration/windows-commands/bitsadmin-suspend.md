---
title: bitsadmin suspend
description: Tema de comandos de Windows para **bitsadmin suspender**, que suspende el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42ed83d4dbf8c3d982c5c186b440cf17997903c9
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123154"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Suspende el trabajo especificado. Si suspendió el trabajo por error, puede usar el modificador de la [reanudación de bitsadmin](bitsadmin-resume.md) para reiniciar el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se suspende el trabajo denominado *myDownloadJob*.


```
C:\>bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
