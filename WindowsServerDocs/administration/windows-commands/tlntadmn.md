---
title: tlntadmn
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c321c4ebcaf5fbbae30f6825fef18b478554f665
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884196"
---
# <a name="tlntadmn"></a>tlntadmn

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administra un equipo local o remoto que se está ejecutando el servicio del servidor telnet.   
## <a name="syntax"></a>Sintaxis  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|\<computerName>|Especifica el nombre del servidor al que conectarse. El valor predeterminado es el equipo local.|  
|-u \<UserName > -p \<contraseña >|Especifica las credenciales administrativas para un servidor remoto que desea administrar. Este parámetro es obligatorio si desea administrar un servidor remoto al que ha no iniciado sesión con credenciales administrativas.|  
|start|inicia el servicio del servidor telnet.|  
|stop|Detiene el servicio del servidor telnet|  
|pause|pone en pausa el servicio servidor telnet. No se aceptará ninguna nueva conexión.|  
|Continuar|Reanuda el servicio servidor telnet.|  
|-s {\<SessionID > &#124; todas}|Muestra las sesiones activas de telnet.|  
|-k {\<SessionID > &#124; todas}|Finaliza las sesiones de telnet. Escriba el identificador de sesión para finalizar una sesión específica, o todos para finalizar todas las sesiones.|  
|-m {\<SessionID > &#124; todas}  <Message>|Envía un mensaje a una o varias sesiones. Escriba el identificador de sesión para enviar un mensaje a una sesión específica, o todos para enviar un mensaje a todas las sesiones. Escriba el mensaje que desea enviar entre comillas.|  
|configuración de dom = \<dominio >|Configura el dominio predeterminado para el servidor.|  
|config ctrlakeymap = {yes &#124; no}|Especifica si desea que el servidor telnet interprete CTRL+A como ALT. tipo **Sí** al mapa de la tecla de método abreviado o tipo **ningún** para evitar la asignación.|  
|tiempo de espera de configuración = \<hh >:\<mm >:\<ss >|Establece el período de tiempo de espera en horas, minutos y segundos.|  
|configuración timeoutactive = {Sí &#124; ninguna|Permite que el tiempo de espera de sesión inactiva.|  
|config maxfail = \<attempts>|Establece el número máximo de intentos de inicio de sesión antes de desconectar.|  
|configuración maxconn = \<conexiones >|Establece el número máximo de conexiones.|  
|puerto de configuración = < \Number >|Establece el puerto telnet. Debe especificar el puerto con un número entero menor que 1024.|  
|config sec {+ &#124; -}NTLM {+ &#124; -}passwd|Especifica si desea utilizar NTLM, una contraseña o ambos para autenticar los intentos de inicio de sesión. Para usar un determinado tipo de autenticación, escriba un signo más (**+**) antes de ese tipo de autenticación. Para impedir el uso de un determinado tipo de autenticación, escriba un signo menos (**-**) antes de ese tipo de autenticación.|  
|modo de configuración = {consola &#124; secuencia}|Especifica el modo de funcionamiento.|  
|-?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Para mostrar la configuración del servidor, escriba **tlntadmn** sin ningún parámetro.  
-   Para usar el **tlntadmn** comando, debe iniciar sesión en el equipo local con credenciales administrativas. Para administrar un equipo remoto, también debe proporcionar credenciales administrativas para el equipo remoto. Para ello, inicie sesión en el equipo local con una cuenta que tenga credenciales administrativas para el equipo local y el equipo remoto. Si no puede usar este método, puede usar el **-u** y **-p** parámetros para proporcionar credenciales administrativas para el equipo remoto.  

## <a name="BKMK_Examples"></a>Ejemplos  
Configurar el tiempo de espera de sesión inactiva en 30 minutos.  
```  
tlntadmn config timeout=0:30:0  
```  
Mostrar sesiones activas de telnet.  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [telnet Operations Guide](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
