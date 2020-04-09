---
title: bitsadmin getreplydata
description: Tema de comandos de Windows para **bitsadmin getreplydata**, que recupera los datos de la respuesta de carga del servidor en formato hexadecimal para el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb83ca93f8e73445788d926e0d5e6db4c774d759
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850508"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera los datos de la respuesta de carga del servidor en formato hexadecimal para el trabajo.

> [!NOTE]
> Este comando no es compatible con BITS 1,2 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getreplydata <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recuperan los datos de la respuesta de carga para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getreplydata myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)