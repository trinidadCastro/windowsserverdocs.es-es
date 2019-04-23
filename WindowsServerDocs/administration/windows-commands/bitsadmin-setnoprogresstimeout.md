---
title: bitsadmin setnoprogresstimeout
description: Tema de los comandos de Windows para **setnoprogresstimeout bitsadmin** -establece el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produce un error transitorio.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45dd8a7ddfae877984a98db66c742e0af4d18f0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873776"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Establece el período de tiempo, en segundos, que BITS intenta transferir el archivo después de producirse el primer error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|TimeOutvalue|Un número representado en segundos.|

## <a name="remarks"></a>Comentarios

-   El intervalo de tiempo de espera de progreso no comienza cuando el trabajo encuentra un error transitorio.
-   El intervalo de tiempo de espera se detiene o se restablece cuando se ha transferido correctamente un byte de datos.
-   Si ningún intervalo de tiempo de espera de curso supera el *valorDeTiempoDeEspera*, el trabajo se coloca en un estado de error grave.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente no se establece el valor de tiempo de espera de progreso del trabajo denominado *myDownloadJob* en 20 segundos
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)