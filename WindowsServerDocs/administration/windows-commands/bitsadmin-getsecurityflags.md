---
title: bitsadmin getsecurityflags
description: 'Windows Commands topic for **bitsadmin getsecurityflags** : notifica las marcas de seguridad http para la redirección de direcciones URL y las comprobaciones realizadas en el certificado de servidor durante la transferencia.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb53664a6366b411ae1eb9b0fe7c93392d60b542
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381460"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Notifica las marcas de seguridad HTTP para la redirección de direcciones URL y las comprobaciones realizadas en el certificado de servidor durante la transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example
En el ejemplo siguiente se recuperan las marcas securitly de un trabajo denominado *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


