---
title: conjunto de Telnet
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18a49f2e4629b1410a1cec40fe77077c2988dce2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836126"
---
# <a name="telnet-set"></a>telnet: set

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece opciones.   
## <a name="syntax"></a>Sintaxis  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|bsasdel|Envía **retroceso** como un **eliminar**.|  
|crlf|Envía CR & LF (0x0D, 0 x 0A) cuando el **ENTRAR** tecla está presionada. Se conoce como modo de nueva línea.|  
|delasbs|Envía **eliminar** como un **retroceso**.|  
|escape <Character>|Establece el carácter de escape que se usa para escribir el símbolo del sistema de cliente de telnet. El carácter de escape puede ser un único carácter, o puede ser una combinación de la **CTRL** clave además de un carácter. Para establecer una combinación de teclas de control, mantenga presionada la **CTRL** clave mientras se escribe el carácter que desea asignar.|  
|localecho|Activa el eco local.|  
|archivo de registro <FileName>|Inicia la sesión de telnet actual en el archivo local. El registro comienza automáticamente al establecer esta opción.|  
|logging|Activa el registro. Si no se establece ningún archivo de registro, aparece un mensaje de error.|  
|Modo {consola &#124; pantalla}|Establece el modo de operación.|  
|ntlm|Activa la autenticación NTLM.|  
|term {ansi &#124; vt100 &#124; vt52 &#124; vtnt}|Establece el tipo de terminal.|  
|?|Muestra ayuda para este comando.|  
## <a name="remarks"></a>Comentarios  
1.  Puede usar el **unset** comando para desactivar una opción que se configuró anteriormente.  
2.  En versiones no inglesas de telnet, el **conjunto de códigos** <option> está disponible. **Conjunto de códigos** <option> establece el código actual establecido en una opción, que puede ser cualquiera de las siguientes acciones: **shift JIS**, **EUC Japonés**, **JIS Kanji**, **JIS Kanji (78)**, **DEC Kanji**, **NEC Kanji**. Debe establecer el mismo código que se establece en el equipo remoto.  
## <a name="BKMK_Examples"></a>Ejemplos  
Establece el archivo de registro y comenzar a registrar para la tnlog.txt de archivos local  
```  
set logfile tnlog.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
