---
title: bitsadmin removeclientcertificate
description: Windows Commands topic for **bitsadmin removeclientcertificate**, que quita el certificado de cliente del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38cf00dc48ff036e7d710fb7436f00fd9381dd0b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849878"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

Quita el certificado de cliente del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se quita el certificado de cliente del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)