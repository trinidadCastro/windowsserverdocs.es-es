---
title: pushprinterconnections
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5941b1eba55ce7524946f3257c093d409ef7d773
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837078"
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
|<-Registro >|Escribe un archivo de registro de depuración por usuario en% Temp o escribe un registro de depuración por máquina en%windir%\temp.|
|<-? >|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Esta utilidad se usa en scripts de inicio de equipo o de inicio de sesión de usuario, y no se debe ejecutar desde la línea de comandos.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Implementar impresoras mediante directiva de grupo](https://go.microsoft.com/fwlink/?LinkId=230627)