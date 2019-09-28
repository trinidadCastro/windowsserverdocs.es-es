---
title: tlntadmn
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1218a0238f90403edcd04db447ceeac1e10a1fdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385731"
---
# <a name="tlntadmn"></a>tlntadmn

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administra un equipo local o remoto que ejecuta el servicio del servidor Telnet.   
## <a name="syntax"></a>Sintaxis  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>Parámetros  

|                   Parámetro                    |                                                                                                                                                       Descripción                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                @no__t 0computerName >                 |                                                                                                                    Especifica el nombre del servidor al que se va a conectar. El valor predeterminado es el equipo local.                                                                                                                    |
|         -u \<UserName >-p \<Password >          |                                                Especifica las credenciales administrativas para un servidor remoto que desea administrar. Este parámetro es necesario si desea administrar un servidor remoto en el que no ha iniciado sesión con credenciales administrativas.                                                |
|                     start                      |                                                                                                                                            inicia el servicio del servidor Telnet.                                                                                                                                             |
|                      stop                      |                                                                                                                                             Detiene el servicio del servidor Telnet                                                                                                                                              |
|                     pause                      |                                                                                                                          pausa el servicio del servidor Telnet. No se aceptarán nuevas conexiones.                                                                                                                          |
|                    funcionando                    |                                                                                                                                            Reanuda el servicio del servidor Telnet.                                                                                                                                            |
|          -s {\<SessionID > &#124; todos}          |                                                                                                                                             Muestra las sesiones activas de Telnet.                                                                                                                                             |
|          -k {\<SessionID > &#124; All}          |                                                                                                        Finaliza las sesiones de Telnet. Escriba el identificador de sesión para finalizar una sesión específica o escriba todo para finalizar todas las sesiones.                                                                                                         |
|    -m {\<SessionID > &#124; todos} <Message>     |                                                   Envía un mensaje a una o más sesiones. Escriba el identificador de sesión para enviar un mensaje a una sesión específica o escriba todo para enviar un mensaje a todas las sesiones. Escriba el mensaje que desea enviar entre comillas.                                                   |
|             config Dom = \<Domain >             |                                                                                                                                      Configura el dominio predeterminado para el servidor.                                                                                                                                       |
|      config ctrlakeymap = {Yes &#124; no}      |                                                                                     Especifica si desea que el servidor Telnet interprete CTRL + A como ALT. Escriba **sí** para asignar la tecla de método abreviado o escriba **no** para evitar la asignación.                                                                                     |
|       config timeout = \<hh >: \<mm >: \<SS >       |                                                                                                                                 Establece el período de tiempo de espera en horas, minutos y segundos.                                                                                                                                 |
|     config timeoutactive = {sí &#124; no      |                                                                                                                                            Habilita el tiempo de espera de sesión inactiva.                                                                                                                                             |
|          config maxfail = \<attempts >          |                                                                                                                          Establece el número máximo de intentos de inicio de sesión incorrectos antes de desconectar.                                                                                                                          |
|        config maxconn = \<Connections >         |                                                                                                                                         Establece el número máximo de conexiones.                                                                                                                                          |
|            Puerto de configuración = < \ número >             |                                                                                                                    Establece el puerto Telnet. Debe especificar el puerto con un entero inferior a 1024.                                                                                                                    |
| config sec {+ &#124; -} NTLM {+ &#124; -} passwd | Especifica si se desea usar NTLM, una contraseña o ambas para autenticar los intentos de inicio de sesión. Para usar un tipo de autenticación determinado, escriba un signo más ( **+** ) antes de ese tipo de autenticación. Para evitar el uso de un tipo determinado de autenticación, escriba un signo menos ( **-** ) antes de ese tipo de autenticación. |
|     modo de configuración = { &#124; streaming de consola}      |                                                                                                                                             Especifica el modo de funcionamiento.                                                                                                                                             |
|                       -?                       |                                                                                                                                           Muestra la ayuda en el símbolo del sistema.                                                                                                                                           |

## <a name="remarks"></a>Comentarios  
-   Para mostrar la configuración del servidor, escriba **tlntadmn** sin parámetros.  
-   Para usar el comando **tlntadmn** , debe iniciar sesión en el equipo local con credenciales administrativas. Para administrar un equipo remoto, también debe proporcionar credenciales administrativas para el equipo remoto. Para ello, inicie sesión en el equipo local con una cuenta que tenga credenciales administrativas tanto para el equipo local como para el equipo remoto. Si no puede usar este método, puede usar los parámetros **-u** y **-p** para proporcionar credenciales administrativas para el equipo remoto.  

## <a name="BKMK_Examples"></a>Example  
Configure el tiempo de espera de sesión inactiva en 30 minutos.  
```  
tlntadmn config timeout=0:30:0  
```  
Mostrar sesiones activas de Telnet.  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Guía de operaciones de Telnet](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
