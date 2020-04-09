---
title: waitfor
description: Comandos de Windows tema para WAITFOR, que envía o espera una señal en un sistema. **WAITFOR** se usa para sincronizar los equipos a través de una red.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4542fc9d231b8150ab89e07e173d9671d6b7a3f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829938"
---
# <a name="waitfor"></a>waitfor



Envía o espera una señal en un sistema. **WAITFOR** se usa para sincronizar los equipos a través de una red.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

### <a name="parameters"></a>Parámetros

|       Parámetro       |                                                                                         Descripción                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s \<equipo >     | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificados en el comando. |
| /u [\<\]de > de dominio <User> |                              Ejecuta el script con las credenciales de la cuenta de usuario especificada. De forma predeterminada, **WAITFOR** usa las credenciales del usuario actual.                               |
|   /p [\<contraseña >]    |                                                    Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                     |
|          /Si          |                                                                        Envía la señal especificada a través de la red.                                                                        |
|     /t \<tiempo de espera >     |                                              Especifica el número de segundos que hay que esperar una señal. De forma predeterminada, **WAITFOR** espera indefinidamente.                                               |
|     \<SignalName >     |                                                Especifica la señal que **WAITFOR** espera o envía. *SignalName* no distingue entre mayúsculas y minúsculas.                                                 |
|          /?           |                                                                             Muestra la Ayuda en el símbolo del sistema.                                                                             |

## <a name="remarks"></a>Comentarios

-   Los nombres de señal no pueden superar los 225 caracteres. Los caracteres válidos son a-z, A-Z, 0-9 y el juego de caracteres extendidos ASCII (128-255).
-   Si no usa **/s**, la señal se difundirá a todos los sistemas de un dominio. Si utiliza **/s**, la señal se envía solo al sistema especificado.
-   Puede ejecutar varias instancias de **WAITFOR** en un único equipo, pero cada instancia de **WAITFOR** debe esperar una señal diferente. Solo una instancia de **WAITFOR** puede esperar una señal determinada en un equipo determinado.
-   Puede activar una señal manualmente mediante la opción de línea de comandos **/si** .
-   **WAITFOR** solo se ejecuta en Windows XP y en los servidores que ejecutan un sistema operativo windows Server 2003, pero puede enviar señales a cualquier equipo que ejecute un sistema operativo Windows.
-   Los equipos solo pueden recibir señales si están en el mismo dominio que el equipo que envía la señal.
-   Puede usar **WAITFOR** al probar las compilaciones de software. Por ejemplo, el equipo de compilación puede enviar una señal a varios equipos que ejecuten **WAITFOR** después de que la compilación se haya completado correctamente. Al recibir la señal, el archivo por lotes que incluye **WAITFOR** puede indicar a los equipos que inicien inmediatamente la instalación de software o la ejecución de pruebas en la compilación compilada.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para esperar hasta que se reciba la señal espresso\build007, escriba:
```
waitfor espresso\build007
```
De forma predeterminada, **WAITFOR** espera indefinidamente una señal.

Para esperar 10 segundos a que se reciba la señal espresso\compile007 antes de que se agote el tiempo de espera, escriba:
```
waitfor /t 10 espresso\build007
```
Para activar manualmente la señal espresso\build007, escriba:
```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)