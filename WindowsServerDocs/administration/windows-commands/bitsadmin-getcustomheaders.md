---
title: bitsadmin getcustomheaders
description: Tema de comandos de Windows para **bitsadmin getcustomheaders**, que recupera los encabezados HTTP personalizados del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80a3d1230fd541f0b986434ce373da4c8bb0c761
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850738"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

Recupera los encabezados HTTP personalizados del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se obtienen los encabezados personalizados para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)