---
title: pushprinterconnections
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94ba2d09a3e67248a393350e46e971aa8b24b00d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722764"
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

## <a name="remarks"></a>Observaciones

Esta utilidad se usa en scripts de inicio de equipo o de inicio de sesión de usuario, y no se debe ejecutar desde la línea de comandos.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Implementar impresoras mediante directiva de grupo](https://go.microsoft.com/fwlink/?LinkId=230627)