---
title: driverquery
description: Artículo de referencia para el comando DRIVERQUERY, que permite a un administrador mostrar una lista de los controladores de dispositivos instalados y sus propiedades.
ms.topic: reference
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbd8ca2de7f15a5b5fb8682dae3a3aa2e105d7cd
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030803"
---
# <a name="driverquery"></a>driverquery

Permite a un administrador mostrar una lista de los controladores de dispositivos instalados y sus propiedades. Si se usa sin parámetros, **DRIVERQUERY** se ejecuta en el equipo local.

## <a name="syntax"></a>Sintaxis

```
driverquery [/s <system> [/u [<domain>\]<username> [/p <password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| modificado `<system>` | Especifica el nombre o la dirección IP de un equipo remoto. No use barras diagonales inversas. La opción predeterminada es el equipo local. |
| /u `[<domain>]<username>` | Ejecuta el comando con las credenciales de la cuenta de usuario de acuerdo con lo especificado por el *usuario* o *dominio\usuario*. De forma predeterminada, */s* usa las credenciales del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. **/u** no se puede usar a menos que se especifique **/s** . |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . **/p** no se puede usar a menos que se especifique **/u** . |
| /FO Table | Aplica al resultado un formato de tabla. Este es el valor predeterminado. |
| /FO (lista) | Da formato a la salida como una lista. |
| /FO CSV | Da formato al resultado con valores separados por comas. |
| /NH | Omite la fila de encabezado de la información de controlador mostrada. No es válido si el parámetro **/FO** está establecido en **List**. |
| /v | Muestra la salida detallada. **/v** no es válido para los controladores firmados. |
| /Si | Proporciona información acerca de los controladores firmados. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para mostrar una lista de los controladores de dispositivos instalados en el equipo local, escriba:

```
driverquery
```

Para mostrar el resultado en un formato de valores separados por comas (CSV), escriba:

```
driverquery /fo csv
```

Para ocultar la fila de encabezado en la salida, escriba:

```
driverquery /nh
```

Para usar el comando **DRIVERQUERY** en un servidor remoto denominado *server1* con las credenciales actuales en el equipo local, escriba:

```
driverquery /s server1
```

Para usar el comando **DRIVERQUERY** en un servidor remoto denominado *server1* con las credenciales de *user1* en el dominio *maindom*, escriba:

```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)