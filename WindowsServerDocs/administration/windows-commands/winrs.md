---
title: winrs
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9224e2572d7d5efded149cd113730dabc1624299
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843616"
---
# <a name="winrs"></a>winrs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administración remota de Windows le permite administrar y ejecutar programas de forma remota.   
## <a name="syntax"></a>Sintaxis  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[/ remoto]\:\<punto de conexión >|Especifica el extremo de destino con un nombre NetBIOS o la conexión estándar:<br /><br />-   <url>: [\<transport>://]\<target>[:\<port>]<br /><br />Si no se especifica, **/r:localhost** se utiliza.|  
|/unencrypted]|Especifica que no se cifrarán los mensajes al shell remoto. Esto es útil para solucionar problemas o cuando el tráfico de red ya está cifrado con **ipsec**, o cuando se aplica la seguridad física.<br /><br />De forma predeterminada, los mensajes se cifran mediante claves de Kerberos o NTLM.<br /><br />Esta opción de línea de comandos se omite cuando se selecciona el transporte HTTPS.|  
|Section]:\<nombre de usuario >|Especifica el nombre de usuario en línea de comandos.<br /><br />Si no se especifica, la herramienta usará la autenticación Negotiate o símbolo del sistema para el nombre.<br /><br />Si **Section** se especifica, **/Password** también debe especificarse.|  
|/password]:\<password>|Especifica la contraseña en línea de comandos.<br /><br />Si **/Password** no se especifica, pero **Section** es, la herramienta pedirá la contraseña.<br /><br />Si **/Password** se especifica, **Section** también debe especificarse.|  
|/ timeout:\<segundos >|Esta opción está en desuso.|  
|/ directory:\<ruta de acceso >|Especifica el directorio de inicio para shell remoto.<br /><br />Si no se especifica, se iniciará el shell remoto en el directorio principal del usuario definido por la variable de entorno **% USERPROFILE %**.|  
|/Environment:\<cadena > =<value>|Especifica una variable de entorno solo estará establecida cuando se inicia el shell, que permite cambiar el entorno predeterminado para el shell.<br /><br />Varias apariciones de este modificador deben utilizarse para especificar varias variables de entorno.|  
|/noecho|Especifica que eco debe deshabilitarse. Esto puede ser necesario para asegurarse de que no se muestran las respuestas del usuario a las solicitudes remotas localmente.<br /><br />De forma predeterminada el eco es "on".|  
|/noprofile|Especifica que no se debe cargar el perfil del usuario.<br /><br />De forma predeterminada, el servidor intentará cargar el perfil de usuario.<br /><br />Si el usuario remoto no es un administrador local en el sistema de destino, esta opción será necesaria (el valor predeterminado se producirá error).|  
|/allowdelegate|Especifica que las credenciales del usuario se pueden usar para tener acceso a un recurso remoto, por ejemplo, se encuentra en un equipo diferente que el extremo de destino.|  
|/Compression|Activar la compresión.  Las instalaciones anteriores en los equipos remotos pueden no admitir la compresión por lo que está desactivada de forma predeterminada.<br /><br />Valor predeterminado es off, ya que las instalaciones anteriores en los equipos remotos no admitan compresión.|  
|/usessl|Usar una conexión SSL al usar un punto de conexión remota.  Especificar esto en lugar del transporte **https:** usará el valor predeterminado **WinRM** el puerto predeterminado.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Todas las opciones de línea de comandos aceptan el formato corto o largo. Por ejemplo, ambos **/r** y **/remote** son válidos.  
-   Para finalizar la **/remote** comando, el usuario puede escribir **Ctrl-C** o **CTRL+INTER**, que se enviará al shell remoto. El segundo **Ctrl-C** forzar la terminación de **winrs.exe**.  
-   Para administrar activos shells remotos o configuración winrs, utilice la herramienta de WinRM.  El URI es el alias para administrar los shells activos **shellcmd/**.  El URI de alias para la configuración de winrs es **winrm/config/winrs**.  

## <a name="BKMK_Examples"></a>Ejemplos  
```  
winrs /r:https://contoso.com command  
```  
```  
winrs /r:contoso.com /usessl command  
```  
```  
winrs /r:myserver command  
```  
```  
winrs /r:http://127.0.0.1 command  
```  
```  
winrs /r:http://169.51.2.101:80 /unencrypted command  
```  
```  
winrs /r:https://[::FFFF:129.144.52.38] command  
```  
```  
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command  
```  
```  
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig  
```  
```  
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd  
```  
```  
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789  
```  
```  
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
