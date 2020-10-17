---
title: WAITFOR
description: Artículo de referencia para el comando WAITFOR, que envía o espera una señal en un sistema.
ms.topic: reference
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 157861a02abe8fbf851a6ef12460b257a7628803
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156128"
---
# <a name="waitfor"></a>WAITFOR

Envía o espera una señal en un sistema. Este comando se usa para sincronizar los equipos a través de una red.

## <a name="syntax"></a>Sintaxis

```
waitfor [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] /si <signalname>
waitfor [/t <timeout>] <signalname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificados en el comando. Si no usa este parámetro, la señal se difundirá a todos los sistemas de un dominio. Si usa este parámetro, la señal se envía solo al sistema especificado. |
| /u `[<domain>]<user>` | Ejecuta el script con las credenciales de la cuenta de usuario especificada. De forma predeterminada, **WAITFOR** usa las credenciales del usuario actual. |
| /p `[\<password>]` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /Si | Envía la señal especificada a través de la red. Este parámetro también le permite activar una señal manualmente. |
| /t `<timeout>` | Especifica el número de segundos que hay que esperar una señal. De forma predeterminada, **WAITFOR** espera indefinidamente. |
| `<signalname>` | Especifica la señal que **WAITFOR** espera o envía. Este parámetro no distingue entre mayúsculas y minúsculas y no puede superar los 225 caracteres. Los caracteres válidos son a-z, A-Z, 0-9 y el juego de caracteres extendidos ASCII (128-255). |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Puede ejecutar varias instancias de **WAITFOR** en un único equipo, pero cada instancia de **WAITFOR** debe esperar una señal diferente. Solo una instancia de **WAITFOR** puede esperar una señal determinada en un equipo determinado.

- Los equipos solo pueden recibir señales si están en el mismo dominio que el equipo que envía la señal.

- Puede usar este comando al probar las compilaciones de software. Por ejemplo, el equipo de compilación puede enviar una señal a varios equipos que ejecuten **WAITFOR** después de que la compilación se haya completado correctamente. Al recibir la señal, el archivo por lotes que incluye **WAITFOR** puede indicar a los equipos que inicien inmediatamente la instalación de software o la ejecución de pruebas en la compilación compilada.

## <a name="examples"></a>Ejemplos

Para esperar hasta que se reciba la señal *espresso\build007* , escriba:

```
waitfor espresso\build007
```

De forma predeterminada, **WAITFOR** espera indefinidamente una señal.

Para esperar *10 segundos* a que se reciba la señal *espresso\compile007* antes de que se agote el tiempo de espera, escriba:

```
waitfor /t 10 espresso\build007
```

Para activar manualmente la señal *espresso\build007* , escriba:

```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
