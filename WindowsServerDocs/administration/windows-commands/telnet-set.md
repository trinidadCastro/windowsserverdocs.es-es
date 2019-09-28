---
title: Telnet set
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e39e2812edc9cd5f169a046def26beebda1d007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383622"
---
# <a name="telnet-set"></a>Telnet: establecer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece opciones.   
## <a name="syntax"></a>Sintaxis  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Parámetros  

|                    Parámetro                     |                                                                                                                                              Descripción                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Envía el **retroceso** como una **eliminación**.                                                                                                                                  |
|                       CRLF                       |                                                                                                        Envía CR & LF (0x0D, 0x 0A) cuando se presiona la tecla **entrar** . Se conoce como nuevo modo de línea.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Envía **Delete** como **retroceso**.                                                                                                                                  |
|                escape <Character>                | Establece el carácter de escape que se usa para especificar el símbolo del sistema del cliente Telnet. El carácter de escape puede ser un carácter único, o puede ser una combinación de la tecla **Ctrl** más un carácter. Para establecer una combinación de teclas de control, mantenga presionada la tecla **Ctrl** mientras escribe el carácter que desea asignar. |
|                    localecho                     |                                                                                                                                         Activa el eco local.                                                                                                                                          |
|                logfile <FileName>                |                                                                                               Registra la sesión de Telnet actual en el archivo local. El registro se inicia automáticamente al establecer esta opción.                                                                                               |
|                     logging                      |                                                                                                                  Activa el registro. Si no se establece ningún archivo de registro, aparece un mensaje de error.                                                                                                                   |
|           modo {pantalla &#124; de la consola}           |                                                                                                                                       Establece el modo de operación.                                                                                                                                        |
|                       NTLM                       |                                                                                                                                     Activa la autenticación NTLM.                                                                                                                                     |
| término {ANSI &#124; VT100 &#124; vt52 &#124; VTNT} |                                                                                                                                        Establece el tipo de terminal.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Muestra ayuda para este comando.                                                                                                                                    |

## <a name="remarks"></a>Comentarios  
1. Puede usar el comando **unset** para desactivar una opción que se estableció previamente.  
2. En las versiones de telnet que no estén en Inglés **, el** Co<option> está disponible. Conjunto de **códigos** <option> establece el código actual establecido en una opción, que puede ser cualquiera de los siguientes: **Shift JIS**, **japonés EUC**, **JIS kanji**, **JIS kanji (78)** , **Dec kanji**, **NEC**kanji. Debe establecer el mismo conjunto de código en el equipo remoto.  
   ## <a name="BKMK_Examples"></a>Example  
   Establezca el archivo de registro y comience a registrar el archivo local tnlog. txt.  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>Referencias adicionales  
3. [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
