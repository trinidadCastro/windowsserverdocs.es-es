---
title: bitsadmin resume
description: Tema de comandos de Windows para la **reanudación de bitsadmin**, que activa un trabajo nuevo o suspendido en la cola de transferencia.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a3f464ba00c5cc233c42a40c063372dc0d584e9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849758"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Activa un trabajo nuevo o suspendido en la cola de transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se reanuda el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)