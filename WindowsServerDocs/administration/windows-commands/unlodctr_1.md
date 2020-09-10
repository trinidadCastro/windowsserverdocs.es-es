---
title: unlodctr
description: Artículo de referencia para Unlodctr, que quita nombres de contadores de rendimiento y texto explicativo de un servicio o controlador de dispositivo del registro del sistema
ms.topic: reference
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e0494d0b4509c4b75af9ca8473e3ae65353e02c1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626561"
---
# <a name="unlodctr"></a>unlodctr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita los nombres de los **contadores de rendimiento** y el texto **explicativo** de un servicio o controlador de dispositivo del registro del sistema.

## <a name="syntax"></a>Sintaxis
```
Unlodctr <DriverName>
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<DriverName>|quita la configuración del nombre del contador de rendimiento y el texto explicativo del controlador o servicio <DriverName> del registro de Windows Server 2003.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
> [!WARNING]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, <DriverName> ).

## <a name="examples"></a>Ejemplos
Para quitar la configuración del registro de rendimiento actual y el texto de explicación del contador para el servicio de Protocolo simple de transferencia de correo (SMTP):
```
unlodctr SMTPSVC
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

