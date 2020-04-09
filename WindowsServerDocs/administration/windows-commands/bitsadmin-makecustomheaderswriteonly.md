---
title: bitsadmin makecustomheaderswriteonly
description: Tema de comandos de Windows para **bitsadmin makecustomheaderswriteonly**, que hace que los encabezados HTTP personalizados de un trabajo sean de solo escritura.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9183b1b5de51020c5c6d2efad2c0a788d158a183
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850248"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Hacer que los encabezados HTTP personalizados de un trabajo sean de solo escritura.

> [!Important]
> Esta acción no se puede deshacer.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se hace que los encabezados HTTP personalizados sean de solo escritura para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)