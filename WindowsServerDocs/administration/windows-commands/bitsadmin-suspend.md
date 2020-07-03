---
title: bitsadmin suspend
description: Artículo de referencia del comando bitsadmin Suspend, que suspende el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42257d31dbada2badc12b44b6375702ac41a91ca
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927478"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Suspende el trabajo especificado. Si suspendió el trabajo por error, puede usar el modificador de la [reanudación de bitsadmin](bitsadmin-resume.md) para reiniciar el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="example"></a>Ejemplo

Para suspender el trabajo denominado *myDownloadJob*:


```
bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de reanudación de bitsadmin](bitsadmin-resume.md)

- [bitsadmin (comando)](bitsadmin.md)
