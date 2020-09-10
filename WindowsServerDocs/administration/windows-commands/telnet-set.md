---
title: telnet set
description: Artículo de referencia para telnet Set, que establece opciones.
ms.topic: reference
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 858a73e8f4a379361ac7158b70d0718abc800410
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640837"
---
# <a name="telnet-set"></a>Telnet: establecer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece opciones.

## <a name="syntax"></a>Sintaxis
```
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]
```
#### <a name="parameters"></a>Parámetros

|                    Parámetro                     |                                                                                                                                              Descripción                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Envía el **retroceso** como una **eliminación**.                                                                                                                                  |
|                       CRLF                       |                                                                                                        Envía CR & LF (0x0D, 0x 0A) cuando se presiona la tecla **entrar** . Se conoce como nuevo modo de línea.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Envía **Delete** como **retroceso**.                                                                                                                                  |
|                salida <Character>                | Establece el carácter de escape que se usa para especificar el símbolo del sistema del cliente Telnet. El carácter de escape puede ser un carácter único, o puede ser una combinación de la tecla **Ctrl** más un carácter. Para establecer una combinación de teclas de control, mantenga presionada la tecla **Ctrl** mientras escribe el carácter que desea asignar. |
|                    localecho                     |                                                                                                                                         Activa el eco local.                                                                                                                                          |
|                MSDTC <FileName>                |                                                                                               Registra la sesión de Telnet actual en el archivo local. El registro se iniciará automáticamente al establecer esta opción.                                                                                               |
|                     logging                      |                                                                                                                  Activa el registro. Si no se establece ningún archivo de registro, aparece un mensaje de error.                                                                                                                   |
|           modo {pantalla de &#124; de consola}           |                                                                                                                                       Establece el modo de operación.                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     Activa la autenticación NTLM.                                                                                                                                     |
| término {ANSI &#124; VT100 &#124; vt52 &#124; VTNT} |                                                                                                                                        Establece el tipo de terminal.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Muestra ayuda para este comando.                                                                                                                                    |

## <a name="remarks"></a>Observaciones
1. Puede usar el comando **unset** para desactivar una opción que se estableció previamente.
2. En las versiones de telnet que no estén en inglés, el tipo de **códigos** <option> está disponible. **Codeset** <option> establece el código actual establecido en una opción, que puede ser cualquiera de los siguientes: **Shift JIS**, **japonés EUC**, **JIS kanji**, **JIS kanji (78)**, **Dec kanji**, **NEC kanji**. Debe establecer el mismo conjunto de código en el equipo remoto.
   ## <a name="examples"></a>Ejemplos
   Establezca el archivo de registro y comience a registrar el archivo local tnlog.txt
   ```
   set logfile tnlog.txt
   ```
   ## <a name="additional-references"></a>Referencias adicionales
3. - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
