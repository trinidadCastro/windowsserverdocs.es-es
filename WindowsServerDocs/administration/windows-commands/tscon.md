---
title: tscon
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8cac59b2f5524df5a82e9c83424fd781f0ef7c8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440931"
---
# <a name="tscon"></a>tscon

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta a otra sesión de un servidor Host de sesión de escritorio remoto (Host de sesión de rd).  
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).  

> [!NOTE]  
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.  

## <a name="syntax"></a>Sintaxis  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|\<SessionID>|Especifica el identificador de la sesión a la que desea conectarse. Si usa el elemento opcional **/dest:** <*SessionName*> parámetro, este es el identificador de la sesión a la que desea conectarse.|  
|\<SessionName>|Especifica el nombre de la sesión a la que desea conectarse.|  
|/dest:\<SessionName>|Especifica el nombre de la sesión actual. Esta sesión se desconectará al conectarse a la nueva sesión.|  
|/password:\<pw>|Especifica la contraseña del usuario que posee la sesión a la que desea conectarse. Esta contraseña es necesaria cuando el usuario que se conecta no es propietario de la sesión.|  
|/ Password: *|solicita la contraseña del usuario que posee la sesión a la que desea conectarse.|  
|/v|Muestra información sobre las acciones que se va a realizar.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Debe tener permiso de acceso de Control total o permiso de acceso especial para conectarse a otra sesión de conexión.  
-   El **/dest:** <*SessionName*> parámetro le permite conectar la sesión de otro usuario a una sesión diferente.  
-   Si no especifica una contraseña en el <*contraseña*> parámetro y la sesión de destino pertenece a un usuario distinto del actual, **tscon** se produce un error.  
-   No se puede conectar a la sesión de consola.  

## <a name="BKMK_examples"></a>Ejemplos  
- Para conectarse a la sesión 12 en el servidor Host de sesión de escritorio remoto actual y desconectar la sesión actual, escriba:  
  ```  
  tscon 12  
  ```  
- Para conectarse a la sesión 23 en el servidor Host de sesión de escritorio remoto actual, mediante el uso de la contraseña micontra y desconectar la sesión actual, escriba:  
  ```  
  tscon 23 /password:mypass  
  ```  
- Para conectar la sesión llamada TERM03 a la sesión TERM05 y, a continuación, desconecte la sesión TERM05, si está conectado, escriba:  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  #### <a name="additional-references"></a>Referencias adicionales  
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  [Servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)  
