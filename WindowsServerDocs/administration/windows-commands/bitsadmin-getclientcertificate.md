---
title: bitsadmin getclientcertificate
description: Tema de comandos de Windows para **bitsadmin getclientcertificate**, que recupera el certificado de cliente del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c29d5c64fd172cfdd2d5d93c5ed22d519077806
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850768"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate

Recupera el certificado de cliente del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el certificado de cliente para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)