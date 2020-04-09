---
title: winrs
description: Temas de comandos de Windows para Winrs, que permite administrar y ejecutar programas de forma remota.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b714f851c981611bbe4d4f26a7b7eed2db7903
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829148"
---
# <a name="winrs"></a>winrs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La administración remota de Windows permite administrar y ejecutar programas de forma remota.   
## <a name="syntax"></a>Sintaxis  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
#### <a name="parameters"></a>Parámetros  

|           Parámetro            |                                                                                                                                                                                    Descripción                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Remote:\<punto de conexión >       |                                                                                          Especifica el extremo de destino mediante un nombre NetBIOS o la conexión estándar:<p>-   <url>: [\<Transport >://]\<destino > [:\<puerto >]<p>Si no se especifica, se usa **/r: localhost** .                                                                                          |
|          /unencrypted          | Especifica que los mensajes al shell remoto no se cifrarán. Esto resulta útil para solucionar problemas o cuando el tráfico de red ya está cifrado mediante **IPSec**, o cuando se aplica la seguridad física.<p>De forma predeterminada, los mensajes se cifran mediante claves Kerberos o NTLM.<p>Esta opción de línea de comandos se omite cuando se selecciona transporte HTTPS. |
|     /username:\<username >      |                                                                                Especifica el nombre de usuario en la línea de comandos.<p>Si no se especifica, la herramienta utilizará la autenticación Negotiate o solicitará el nombre.<p>Si se especifica **/username** , se debe especificar también **/password** .                                                                                 |
|     /Password:\<contraseña >      |                                                                           Especifica la contraseña en la línea de comandos.<p>Si no se especifica **/password** pero **/username** es, la herramienta solicitará la contraseña.<p>Si se especifica **/password** , también se debe especificar **/username** .                                                                            |
|      /timeout:\<segundos >       |                                                                                                                                                                             Esta opción está en desuso.                                                                                                                                                                             |
|       /Directory:\<ruta de acceso >       |                                                                                            Especifica el directorio inicial para el shell remoto.<p>Si no se especifica, el shell remoto se iniciará en el directorio particular del usuario definido por la variable de entorno **% userprofile%** .                                                                                             |
| /Environment:\<cadena > =<value> |                                                                          Especifica una única variable de entorno que se establecerá cuando se inicie el Shell, lo que permite cambiar el entorno predeterminado para el shell.<p>Se deben usar varias repeticiones de este modificador para especificar varias variables de entorno.                                                                          |
|            /noecho             |                                                                                                    Especifica que debe deshabilitarse el eco. Esto puede ser necesario para asegurarse de que las respuestas del usuario a los mensajes remotos no se muestran en modo local.<p>De forma predeterminada, el eco está activado.                                                                                                    |
|           /noprofile           |                                              Especifica que el perfil del usuario no se debe cargar.<p>De forma predeterminada, el servidor intentará cargar el perfil de usuario.<p>Si el usuario remoto no es un administrador local en el sistema de destino, se necesitará esta opción (el valor predeterminado producirá un error).                                               |
|         /allowdelegate         |                                                                                                                  Especifica que las credenciales del usuario se pueden usar para tener acceso a un recurso compartido remoto, por ejemplo, en un equipo diferente al del extremo de destino.                                                                                                                   |
|          /Compression          |                                                                           Active la compresión.  Es posible que las instalaciones anteriores de equipos remotos no admitan la compresión de modo que esté desactivada de forma predeterminada.<p>La configuración predeterminada está desactivada, ya que es posible que las instalaciones anteriores de equipos remotos no admitan la compresión.                                                                           |
|            /usessl             |                                                                                                               Usar una conexión SSL al usar un extremo remoto.  Especificando esto en lugar del **protocolo https:** usará el puerto predeterminado **WinRM** predeterminado.                                                                                                                |
|               /?               |                                                                                                                                                                        Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                                        |

## <a name="remarks"></a>Comentarios  
-   Todas las opciones de línea de comandos aceptan una forma abreviada o una forma larga. Por ejemplo, **/r** y **/Remote** son válidos.  
-   Para terminar el comando **/Remote** , el usuario puede escribir **Ctrl + C** o **Ctrl + Inter**, que se enviará al shell remoto. El segundo **Ctrl + C** forzará la finalización de **Winrs. exe**.  
-   Para administrar shells remotos activos o la configuración de Winrs, use la herramienta WinRM.  El alias del URI para administrar shells activos es **Shell/cmd**.  El alias de URI para la configuración de Winrs es **WinRM/config/Winrs**.  

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
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
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  

