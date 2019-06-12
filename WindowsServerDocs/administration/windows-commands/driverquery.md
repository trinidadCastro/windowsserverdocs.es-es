---
title: driverquery
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 88a59f9da9927bb923418695bc760303c0fb00b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439480"
---
# <a name="driverquery"></a>driverquery



Habilita un administrador mostrar una lista de controladores de dispositivo instalados y sus propiedades. Si se utiliza sin parámetros, **driverquery** se ejecuta en el equipo local.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                                                                                         Descripción                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<system >        |                                                                                      Especifica el nombre o dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el equipo local.                                                                                       |
| /u [\<Domain>\]<Username> | Ejecuta el comando con las credenciales de la cuenta de usuario según lo especificado por *usuario* o *dominio*\*usuario<em>. De forma predeterminada, \* \*/s</em> \* usa las credenciales del usuario que ha iniciado sesión actualmente en el equipo que está emitiendo el comando. **/u** no se puede usar a menos que **/s** se especifica. |
|      /p \<contraseña >       |                                                                           Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro. **/p** no se puede usar a menos que **/u** se especifica.                                                                            |
|        /FO {table         |                                                                                                                                             list                                                                                                                                             |
|            /nh            |                                                                                      Omite la fila de encabezado de la información de controladores que se mostrará. No tiene validez si el **/fo** parámetro está establecido en **lista**.                                                                                      |
|            /v             |                                                                                                               Muestra resultados detallados. **/v** no es válido para los controladores firmados.                                                                                                               |
|            /si            |                                                                                                                          Proporciona información sobre los controladores firmados.                                                                                                                          |
|            /?             |                                                                                                                             Muestra la ayuda en el símbolo del sistema.                                                                                                                             |

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar una lista de controladores de dispositivo instalado en el equipo local, escriba:
```
driverquery 
```
Para mostrar la salida en un formato de valores separados por comas (CSV), escriba:
```
driverquery /fo csv 
```
Para ocultar la fila de encabezado en la salida, escriba:
```
driverquery /nh 
```
Para usar el **driverquery** comando en un servidor remoto denominado **server1** con sus credenciales actuales en el equipo local, escriba:
```
driverquery /s server1
```
Para usar el **driverquery** comando en un servidor remoto denominado **server1** utilizando las credenciales para **user1** en el dominio **maindom**, tipo:
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)