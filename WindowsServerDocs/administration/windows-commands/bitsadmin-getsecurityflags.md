---
title: bitsadmin getsecurityflags
description: Windows Commands topic for **bitsadmin getsecurityflags**, que informa de las marcas de seguridad http para la redirección de direcciones URL y las comprobaciones realizadas en el certificado de servidor durante la transferencia.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 360f8d5514e5251dd9e4a6a6b60335dc34fe4415
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850478"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Notifica las marcas de seguridad HTTP para la redirección de direcciones URL y las comprobaciones realizadas en el certificado de servidor durante la transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recuperan las marcas de seguridad de un trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)