---
title: bitsadmin resume
description: Artículo de referencia del comando bitsadmin resume, que activa un trabajo nuevo o suspendido en la cola de transferencia.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd2a8dcb486c584ef4adaf96a5288a9db32d4553
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927963"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Activa un trabajo nuevo o suspendido en la cola de transferencia. Si reanudó el trabajo por error, o simplemente necesita suspender el trabajo, puede usar el modificador de [suspensión de bitsadmin](bitsadmin-suspend.md) para suspender el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para reanudar el trabajo denominado *myDownloadJob*:

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin comando suspender](bitsadmin-suspend.md)

- [bitsadmin (comando)](bitsadmin.md)
