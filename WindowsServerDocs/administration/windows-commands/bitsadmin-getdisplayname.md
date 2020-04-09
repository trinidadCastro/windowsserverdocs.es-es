---
title: bitsadmin getdisplayname
description: Temas de comandos de Windows para **bitsadmin getDisplayName**, que recupera el nombre para mostrar del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6944dc2b7a63ca986fb285d26796f350c1052295
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850718"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Recupera el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el nombre para mostrar del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)