---
title: tscon
description: Artículo de referencia de tscon, que se conecta a otra sesión en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83f8778d73d022ed41fcab14795eb3cdf41dc6cf
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954947"
---
# <a name="tscon"></a>tscon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Se conecta a otra sesión en un servidor host de sesión Escritorio remoto.



> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]
```
### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<SessionID>|Especifica el identificador de la sesión a la que se desea conectar. Si usa el parámetro opcional **/dest:** < *nombresesión*>, es el identificador de la sesión a la que desea conectarse.|
|\<SessionName>|Especifica el nombre de la sesión a la que desea conectarse.|
|/dest\<SessionName>|Especifica el nombre de la sesión actual. Esta sesión se desconectará cuando se conecte a la nueva sesión.|
|/Password\<pw>|Especifica la contraseña del usuario propietario de la sesión a la que desea conectarse. Esta contraseña es necesaria cuando el usuario que se conecta no es propietario de la sesión.|
|/Password: *|solicita la contraseña del usuario propietario de la sesión a la que desea conectarse.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   Debe tener el permiso de acceso de control total o el permiso de acceso especial Connect para conectarse a otra sesión.
-   El parámetro **/dest:** < *nombresesión*> permite conectar la sesión de otro usuario a otra sesión.
-   Si no especifica una contraseña en el parámetro <*contraseña*> y la sesión de destino pertenece a un usuario que no es el actual, se produce un error en **tscon** .
-   No se puede conectar a la sesión de consola.

## <a name="examples"></a>Ejemplos
- Para conectarse a la sesión 12 en el servidor host de sesión de escritorio remoto actual y desconectar la sesión actual, escriba:
  ```
  tscon 12
  ```
- Para conectarse a la sesión 23 en el servidor host de sesión de escritorio remoto actual, use la contraseña y desconecte la sesión actual, escriba:
  ```
  tscon 23 /password:mypass
  ```
- Para conectar la sesión denominada TERM03 a la sesión denominada TERM05 y, a continuación, desconectar la sesión TERM05, si está conectada, escriba:
  ```
  tscon TERM03 /v /dest:TERM05
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
   [Referencia de comandos de servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
