---
title: bitsadmin listfiles
description: Windows Commands topic for **bitsadmin ListFiles**, que enumera los archivos del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1af11f7876a3d1cd36aa38c7ac26563c01e81ab5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850318"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Enumera los archivos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la lista de archivos del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)