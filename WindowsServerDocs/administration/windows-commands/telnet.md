---
title: telnet
description: Temas de comandos de Windows para telnet, que se comunica con un equipo que ejecuta el servicio del servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b59a3891dd276c6ab0b8e7a8a0a2d11a6b6b55c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833138"
---
# <a name="telnet"></a>telnet

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se comunica con un equipo que ejecuta el servicio del servidor Telnet.
 
## <a name="syntax"></a>Sintaxis
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/a|intente iniciar sesión automáticamente. Igual que la opción/l, excepto que usa el nombre del usuario que ha iniciado sesión actualmente.|
|/e \<EscapeChar >|Carácter de escape que se usa para especificar el símbolo del sistema del cliente Telnet.|
|/f \<nombre de archivo >|Nombre de archivo utilizado para el registro del lado cliente.|
|/l \<nombreDeUsuario >|Especifica el nombre de usuario con el que iniciar sesión en el equipo remoto.|
|/t {VT100 &#124; vt52 &#124; ANSI &#124; VTNT}|Especifica el tipo de terminal. Los tipos de terminal admitidos son VT100, vt52, ANSI y VTNT.|
|\<host > [\<>]|Especifica el nombre de host o la dirección IP del equipo remoto al que se va a conectar y, opcionalmente, el puerto TCP que se va a usar (el valor predeterminado es el puerto TCP 23).|
|/?|Muestra la Ayuda en el símbolo del sistema. Como alternativa, puede escribir/h.|

## <a name="remarks"></a>Comentarios
-   Debe instalar el software cliente de Telnet para poder ejecutar este comando. Para obtener más información, consulte [instalación de Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Puede ejecutar Telnet sin parámetros para especificar el contexto de Telnet, indicado por el símbolo del sistema de telnet (**Microsoft telnet >** ). En el símbolo del sistema de Telnet, puede usar comandos Telnet para administrar el equipo que ejecuta el cliente Telnet.

## <a name="examples"></a><a name=BKMK_Examples></a>Example
Use Telnet para conectarse al equipo que ejecuta el servicio del servidor Telnet en telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Use Telnet para conectarse al equipo que ejecuta el servicio del servidor Telnet en telnet.microsoft.com en el puerto TCP 44 y registre la actividad de la sesión en un archivo local denominado telnetlog. txt.
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Referencias adicionales
-   [Instalación de Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [Referencia técnica de Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
