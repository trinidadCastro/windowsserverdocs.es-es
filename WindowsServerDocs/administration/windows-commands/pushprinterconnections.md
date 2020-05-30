---
title: pushprinterconnections
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ed83b51da5ad7ad0a6b4dc0afd1d6bc30803a98
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222046"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Lee la configuración de la conexión de impresora implementada de directiva de grupo e implementa o quita las conexiones de impresora según sea necesario.

## <a name="syntax"></a>Sintaxis

```
pushprinterconnections <-log> <-?>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|<-registro>|Escribe un archivo de registro de depuración por usuario en% Temp o escribe un registro de depuración por máquina en%windir%\temp.|
|<-? >|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Esta utilidad se usa en scripts de inicio de equipo o de inicio de sesión de usuario, y no se debe ejecutar desde la línea de comandos.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Implementar impresoras mediante directiva de grupo](https://go.microsoft.com/fwlink/?LinkId=230627)