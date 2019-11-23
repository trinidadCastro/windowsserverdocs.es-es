---
title: driverquery
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d44a1be300b7178bc2271187344c2fc4ab8815e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377648"
---
# <a name="driverquery"></a>driverquery



Permite a un administrador mostrar una lista de los controladores de dispositivos instalados y sus propiedades. Si se usa sin parámetros, **DRIVERQUERY** se ejecuta en el equipo local.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                                                                                         Descripción                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<System >        |                                                                                      Especifica el nombre o la dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el equipo local.                                                                                       |
| /u [\<\]de > de dominio <Username> | Ejecuta el comando con las credenciales de la cuenta de usuario según lo especificado por *usuario* o *dominio*\*usuario<em>. De forma predeterminada, \*\*/s</em>\* usa las credenciales del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. **/u** no se puede usar a menos que se especifique **/s** . |
|      /p \<contraseña >       |                                                                           Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . **/p** no se puede usar a menos que se especifique **/u** .                                                                            |
|        /FO {Table         |                                                                                                                                             list                                                                                                                                             |
|            /NH            |                                                                                      Omite la fila de encabezado de la información de controlador mostrada. No es válido si el parámetro **/FO** está establecido en **List**.                                                                                      |
|            /v             |                                                                                                               Muestra la salida detallada. **/v** no es válido para los controladores firmados.                                                                                                               |
|            /Si            |                                                                                                                          Proporciona información acerca de los controladores firmados.                                                                                                                          |
|            /?             |                                                                                                                             Muestra la ayuda en el símbolo del sistema.                                                                                                                             |

## <a name="BKMK_examples"></a>Example

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
Para usar el comando **DRIVERQUERY** en un servidor remoto denominado **server1** con las credenciales actuales en el equipo local, escriba:
```
driverquery /s server1
```
Para usar el comando **DRIVERQUERY** en un servidor remoto denominado **server1** con las credenciales de **user1** en el dominio **maindom**, escriba:
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)