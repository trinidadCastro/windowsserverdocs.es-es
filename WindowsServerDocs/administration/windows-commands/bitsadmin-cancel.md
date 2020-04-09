---
title: bitsadmin cancel
description: Comando comandos de Windows para **bitsadmin cancelar**, que quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c2bdeef824bc269671cc5ae926fb77cd5726c58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850838"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se quita el trabajo *myDownloadJob* de la cola de transferencia.

```
C:\>bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)