---
title: tlntadmn
description: Artículo de referencia para el comando tlntadmn, que administra un equipo local o remoto, que ejecuta el servicio del servidor Telnet.
ms.topic: reference
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9a4de6903d5a9979f45677176d023c694e20cdfd
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156445"
---
# <a name="tlntadmn"></a>tlntadmn

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Administra un equipo local o remoto que ejecuta el servicio del servidor Telnet. Si se usa sin parámetros, **tlntadmn** muestra la configuración actual del servidor.

Este comando requiere que inicie sesión en el equipo local con credenciales administrativas. Para administrar un equipo remoto, también debe proporcionar credenciales administrativas para el equipo remoto. Para ello, inicie sesión en el equipo local con una cuenta que tenga credenciales administrativas tanto para el equipo local como para el equipo remoto. Si no puede usar este método, puede usar los parámetros **-u** y **-p** para proporcionar credenciales administrativas para el equipo remoto.

## <a name="syntax"></a>Sintaxis

```
tlntadmn [<computername>] [-u <username>] [-p <password>] [{start | stop | pause | continue}] [-s {<sessionID> | all}] [-k {<sessionID> | all}] [-m {<sessionID> | all}  <message>] [config [dom = <domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <connections>] [port = <number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<computername>` | Especifica el nombre del servidor al que se va a conectar. La opción predeterminada es el equipo local. |
| -u `<username> -p <password>` | Especifica las credenciales administrativas para un servidor remoto que desea administrar. Este parámetro es necesario si desea administrar un servidor remoto en el que no ha iniciado sesión con credenciales administrativas. |
| start | inicia el servicio del servidor Telnet. |
| stop | Detiene el servicio del servidor Telnet |
| pause | Pausa el servicio del servidor Telnet. No se aceptarán nuevas conexiones. |
| continue | Reanuda el servicio del servidor Telnet. |
| -s `{<sessionID> | all}` | Muestra las sesiones activas de Telnet. |
| -k `{<sessionID> | all}` | Finaliza las sesiones de Telnet. Escriba el identificador de sesión para finalizar una sesión específica o escriba todo para finalizar todas las sesiones. |
| -m `{<sessionID> | all}  <message>` | Envía un mensaje a una o más sesiones. Escriba el identificador de sesión para enviar un mensaje a una sesión específica o escriba todo para enviar un mensaje a todas las sesiones. Escriba el mensaje que desea enviar entre comillas. |
| configuración Dom = `<domain>` | Configura el dominio predeterminado para el servidor. |
| configuración ctrlakeymap = `{yes | no}` | Especifica si desea que el servidor Telnet interprete CTRL + A como ALT. Escriba **sí** para asignar la tecla de método abreviado o escriba **no** para evitar la asignación. |
| tiempo de espera de configuración = `<hh>:<mm>:<ss>` | Establece el período de tiempo de espera en horas, minutos y segundos. |
| configuración timeoutactive = `{yes | no}` | Habilita el tiempo de espera de sesión inactiva. |
| configuración maxfail = `<attempts>` | Establece el número máximo de intentos de inicio de sesión incorrectos antes de desconectar. |
| configuración maxconn = `<connections>` | Establece el número máximo de conexiones. |
| Puerto de configuración = `<number>` | Establece el puerto Telnet. Debe especificar el puerto con un entero inferior a 1024. |
| configuración s `{+ | -}NTLM {+ | -}passwd` | Especifica si se desea usar NTLM, una contraseña o ambas para autenticar los intentos de inicio de sesión. Para usar un tipo de autenticación determinado, escriba un signo más ( **+** ) antes de ese tipo de autenticación. Para evitar el uso de un tipo determinado de autenticación, escriba un signo menos ( **-** ) antes de ese tipo de autenticación. |
| modo de configuración = `{console | stream}` | Especifica el modo de funcionamiento. |
| -? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para configurar el tiempo de espera de sesión inactiva en 30 minutos, escriba:

```
tlntadmn config timeout=0:30:0
```

Para mostrar las sesiones activas de Telnet, escriba:

```
tlntadmn -s
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de operaciones de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753164(v=ws.10))
