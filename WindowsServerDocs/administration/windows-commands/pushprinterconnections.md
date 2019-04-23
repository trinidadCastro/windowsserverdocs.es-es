---
title: pushprinterconnections
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c571d3adffd0e6a28f63f6d821b2524dc055aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873726"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Lee la configuración de conexión de impresora implementadas de directiva de grupo y se implementa o quita las conexiones de impresora según sea necesario.

## <a name="syntax"></a>Sintaxis

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|<-log>|Escribe una por cada archivo de registro de depuración de usuario a % temp o escribe una por cada registro de depuración de la máquina en % windir%\temp.|
|<-?>|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Esta utilidad es para su uso en scripts de inicio de sesión de usuario o de inicio de máquina y no se debe ejecutar desde la línea de comandos.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Implementación de impresoras con directivas de grupo](https://go.microsoft.com/fwlink/?LinkId=230627)