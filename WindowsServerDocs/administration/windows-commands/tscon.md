---
title: tscon
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6e53ea2888b66b9e4fbf026f752acf9803270fc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392365"
---
# <a name="tscon"></a>tscon

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta a otra sesión en un servidor host de sesión de Escritorio remoto (host de sesión de escritorio remoto).  
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).  

> [!NOTE]  
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.  

## <a name="syntax"></a>Sintaxis  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|\<SessionID >|Especifica el identificador de la sesión a la que se desea conectar. Si usa el parámetro opcional **/dest:** <*nombresesión*>, es el identificador de la sesión a la que desea conectarse.|  
|\<nombresesión >|Especifica el nombre de la sesión a la que desea conectarse.|  
|/dest:\<nombresesión >|Especifica el nombre de la sesión actual. Esta sesión se desconectará cuando se conecte a la nueva sesión.|  
|/Password:\<PW >|Especifica la contraseña del usuario propietario de la sesión a la que desea conectarse. Esta contraseña es necesaria cuando el usuario que se conecta no es propietario de la sesión.|  
|/Password: *|solicita la contraseña del usuario propietario de la sesión a la que desea conectarse.|  
|/v|Muestra información acerca de las acciones que se llevan a cabo.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Observaciones  
-   Debe tener el permiso de acceso de control total o el permiso de acceso especial Connect para conectarse a otra sesión.  
-   El parámetro **/dest:** <*nombresesión*> permite conectar la sesión de otro usuario a otra sesión.  
-   Si no especifica una contraseña en el parámetro <*contraseña*> y la sesión de destino pertenece a un usuario que no es el actual, se produce un error en **tscon** .  
-   No se puede conectar a la sesión de consola.  

## <a name="BKMK_examples"></a>Example  
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
  #### <a name="additional-references"></a>Referencias adicionales  
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  [Referencia &#40;de&#41; comandos de Terminal Services de servicios de escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)  
