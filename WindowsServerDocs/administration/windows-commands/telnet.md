---
title: telnet
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1621a10b9b445e87eb6bc16a8d7f52a64c146a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385153"
---
# <a name="telnet"></a>telnet

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se comunica con un equipo que ejecuta el servicio del servidor Telnet. 
## <a name="syntax"></a>Sintaxis
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/a|intente iniciar sesión automáticamente. Igual que la opción/l, excepto que usa el nombre del usuario que ha iniciado sesión actualmente.|
|/e \<EscapeChar >|Carácter de escape que se usa para especificar el símbolo del sistema del cliente Telnet.|
|/f \<FileName >|Nombre de archivo utilizado para el registro del lado cliente.|
|/l \<UserName >|Especifica el nombre de usuario con el que iniciar sesión en el equipo remoto.|
|/t {VT100 &#124; vt52 &#124; ANSI &#124; VTNT}|Especifica el tipo de terminal. Los tipos de terminal admitidos son VT100, vt52, ANSI y VTNT.|
|\<Host > [\<Port >]|Especifica el nombre de host o la dirección IP del equipo remoto al que se va a conectar y, opcionalmente, el puerto TCP que se va a usar (el valor predeterminado es el puerto TCP 23).|
|/?|Muestra la ayuda en el símbolo del sistema. Como alternativa, puede escribir/h.|

## <a name="remarks"></a>Comentarios
-   Debe instalar el software cliente de Telnet para poder ejecutar este comando. Para obtener más información, consulte [instalación de Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Puede ejecutar Telnet sin parámetros para especificar el contexto de Telnet, indicado por el símbolo del sistema de telnet (**Microsoft telnet >** ). En el símbolo del sistema de Telnet, puede usar comandos Telnet para administrar el equipo que ejecuta el cliente Telnet.

## <a name="BKMK_Examples"></a>Example
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
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
