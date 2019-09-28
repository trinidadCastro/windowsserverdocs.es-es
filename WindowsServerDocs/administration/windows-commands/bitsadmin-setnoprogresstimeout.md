---
title: bitsadmin setnoprogresstimeout
description: 'Windows Commands topic for **bitsadmin setnoprogresstimeout** : establece el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produzca un error transitorio.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761d0d76a2c70af9d4ad68aa564c1a9816691d0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380501"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Establece el período de tiempo, en segundos, que BITS intenta transferir el archivo después de que se produzca el primer error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|TimeOutvalue|Número representado en segundos.|

## <a name="remarks"></a>Comentarios

-   El intervalo de tiempo de espera de no progreso comienza cuando el trabajo encuentra un error transitorio.
-   El intervalo de tiempo de espera se detiene o se restablece cuando se transfiere correctamente un byte de datos.
-   Si ningún intervalo de tiempo de espera de progreso supera el *TimeOutvalue*, el trabajo se coloca en un estado de error irrecuperable.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece el valor de tiempo de espera sin progreso para el trabajo denominado *myDownloadJob* en 20 segundos.
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)