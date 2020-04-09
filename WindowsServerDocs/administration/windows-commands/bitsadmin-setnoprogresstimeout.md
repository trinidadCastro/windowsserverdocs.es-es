---
title: bitsadmin setnoprogresstimeout
description: Windows Commands topic for bitsadmin setnoprogresstimeout, que establece el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produzca un error transitorio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 544a6c73f29684bc4091ec05fa28016fbc718bb2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849358"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Establece el período de tiempo, en segundos, que BITS intenta transferir el archivo después de que se produzca el primer error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|TimeOutvalue|Número representado en segundos.|

## <a name="remarks"></a>Comentarios

-   El intervalo de tiempo de espera de no progreso comienza cuando el trabajo encuentra un error transitorio.
-   El intervalo de tiempo de espera se detiene o se restablece cuando se transfiere correctamente un byte de datos.
-   Si ningún intervalo de tiempo de espera de progreso supera el *TimeOutvalue*, el trabajo se coloca en un estado de error irrecuperable.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece el valor de tiempo de espera sin progreso para el trabajo denominado *myDownloadJob* en 20 segundos.
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)