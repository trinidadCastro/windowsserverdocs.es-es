---
title: waitfor
description: Artículo de referencia para WAITFOR, que envía o espera una señal en un sistema. **WAITFOR** se usa para sincronizar los equipos a través de una red.
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e061c36f7cdf949ea76d548a4ed804a0e12169bf
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892249"
---
# <a name="waitfor"></a>waitfor



Envía o espera una señal en un sistema. **WAITFOR** se usa para sincronizar los equipos a través de una red.



## <a name="syntax"></a>Sintaxis

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

### <a name="parameters"></a>Parámetros

|       Parámetro       |                                                                                         Descripción                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    modificado\<Computer>     | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificados en el comando. |
| 5.50\<Domain>\]<User> |                              Ejecuta el script con las credenciales de la cuenta de usuario especificada. De forma predeterminada, **WAITFOR** usa las credenciales del usuario actual.                               |
|   /p [ \<Password> ]    |                                                    Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                     |
|          /Si          |                                                                        Envía la señal especificada a través de la red.                                                                        |
|     /t\<Timeout>     |                                              Especifica el número de segundos que hay que esperar una señal. De forma predeterminada, **WAITFOR** espera indefinidamente.                                               |
|     \<SignalName>     |                                                Especifica la señal que **WAITFOR** espera o envía. *SignalName* no distingue entre mayúsculas y minúsculas.                                                 |
|          /?           |                                                                             Muestra la ayuda en el símbolo del sistema.                                                                             |

## <a name="remarks"></a>Observaciones

-   Los nombres de señal no pueden superar los 225 caracteres. Los caracteres válidos son a-z, A-Z, 0-9 y el juego de caracteres extendidos ASCII (128-255).
-   Si no usa **/s**, la señal se difundirá a todos los sistemas de un dominio. Si utiliza **/s**, la señal se envía solo al sistema especificado.
-   Puede ejecutar varias instancias de **WAITFOR** en un único equipo, pero cada instancia de **WAITFOR** debe esperar una señal diferente. Solo una instancia de **WAITFOR** puede esperar una señal determinada en un equipo determinado.
-   Puede activar una señal manualmente mediante la opción de línea de comandos **/si** .
-   **WAITFOR** solo se ejecuta en Windows XP y en los servidores que ejecutan un sistema operativo windows Server 2003, pero puede enviar señales a cualquier equipo que ejecute un sistema operativo Windows.
-   Los equipos solo pueden recibir señales si están en el mismo dominio que el equipo que envía la señal.
-   Puede usar **WAITFOR** al probar las compilaciones de software. Por ejemplo, el equipo de compilación puede enviar una señal a varios equipos que ejecuten **WAITFOR** después de que la compilación se haya completado correctamente. Al recibir la señal, el archivo por lotes que incluye **WAITFOR** puede indicar a los equipos que inicien inmediatamente la instalación de software o la ejecución de pruebas en la compilación compilada.

## <a name="examples"></a>Ejemplos

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