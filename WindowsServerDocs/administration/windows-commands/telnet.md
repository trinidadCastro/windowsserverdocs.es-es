---
title: telnet
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6439a91d82d6d199666629e333d8130bf65a384b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858566"
---
# <a name="telnet"></a>telnet

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se comunica con un equipo que ejecuta el servicio del servidor telnet. 
## <a name="syntax"></a>Sintaxis
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/a|intento de inicio de sesión automático. Igual que/l opción excepto usa tiene iniciada sesión en nombre de usuario s.|
|/e \<EscapeChar>|Escape de carácter que se usa para introducir el símbolo del sistema de cliente de telnet.|
|/f \<nombreDeArchivo >|Nombre de archivo utilizado para el registro del lado cliente.|
|/l \<nombre de usuario >|Especifica el nombre de usuario para iniciar sesión con en el equipo remoto.|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|Especifica el tipo de terminal. Los tipos de terminales admitidos son vt100, vt52, ansi y vtnt.|
|\<Host > [\<puerto >]|Especifica el nombre de host o dirección IP del equipo remoto para conectarse a y, opcionalmente, el puerto TCP que se usará (el valor predeterminado es el puerto TCP 23).|
|/?|Muestra la ayuda en el símbolo del sistema. Como alternativa, puede escribir /h.|

## <a name="remarks"></a>Comentarios
-   Debe instalar el software cliente de telnet para poder ejecutar este comando. Para obtener más información, consulte [instalar telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Puede ejecutar telnet sin parámetros para entrar en el contexto de telnet, indicado por el símbolo del sistema de telnet (**telnet de Microsoft >**). Desde el símbolo del sistema de telnet, puede usar comandos de telnet para administrar el equipo que ejecuta al cliente telnet.

## <a name="BKMK_Examples"></a>Ejemplos
Utilice telnet para conectarse al equipo que ejecuta el servicio servidor telnet en telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Usar telnet para conectarse al equipo que ejecuta el servicio servidor telnet en telnet.microsoft.com en el puerto TCP 44 e iniciar la actividad de la sesión en un archivo local denominado telnetlog.txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Referencias adicionales
-   [Instalación de telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [Referencia técnica de Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
