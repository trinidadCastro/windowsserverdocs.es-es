---
title: bitsadmin getsecurityflags
description: Artículo de referencia del comando bitsadmin getsecurityflags, que informa de las marcas de seguridad HTTP para la redirección de direcciones URL y las comprobaciones realizadas en el certificado de servidor durante la transferencia.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8613f2bd293bdbf7680aa730ec6fc222ffcfe158
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926688"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Notifica las marcas de seguridad HTTP para la redirección de direcciones URL y las comprobaciones realizadas en el certificado de servidor durante la transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar las marcas de seguridad de un trabajo denominado *myDownloadJob*:

```
bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
