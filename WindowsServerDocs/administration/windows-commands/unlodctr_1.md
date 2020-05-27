---
title: unlodctr
description: Tema de referencia de Unlodctr, que quita nombres de contadores de rendimiento y texto explicativo de un servicio o controlador de dispositivo del registro del sistema
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8551b6fc76984b06f28bdda92dcd63791721ec90
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821295"
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

