---
title: telnet
description: Artículo de referencia para el comando telnet, que se comunica con un equipo que ejecuta el servicio del servidor Telnet.
ms.topic: reference
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cf4fa5754aec18662800f4536afd16a427ca0952
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717932"
---
# <a name="telnet"></a>telnet

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Se comunica con un equipo que ejecuta el servicio del servidor Telnet. La ejecución de este comando sin parámetros le permite especificar el contexto de Telnet, como se indica en el símbolo del sistema de telnet (**Microsoft telnet>**). En el símbolo del sistema de Telnet, puede usar comandos Telnet para administrar el equipo que ejecuta el cliente Telnet.

> [!IMPORTANT]
> Debe instalar el software cliente de Telnet para poder ejecutar este comando. Para obtener más información, consulte [instalación de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10)).

## <a name="syntax"></a>Sintaxis

```
telnet [/a] [/e <escapechar>] [/f <filename>] [/l <username>] [/t {vt100 | vt52 | ansi | vtnt}] [<host> [<port>]] [/?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /a | Intenta iniciar sesión automáticamente. Igual que la opción **/l** , salvo que usa el nombre del usuario que ha iniciado sesión actualmente. |
| /e `<escapechar>` | Especifica el carácter de escape que se usa para especificar el símbolo del sistema del cliente Telnet. |
| /f `<filename>` | Especifica el nombre de archivo utilizado para el registro del lado cliente. |
| l `<username>` | Especifica el nombre de usuario con el que iniciar sesión en el equipo remoto. |
| /t `{vt100 | vt52 | ansi | vtnt}` | Especifica el tipo de terminal. Los tipos de terminal admitidos son **VT100**, **vt52**, **ANSI**y **VTNT**. |
| `<host> [<port>]` | Especifica el nombre de host o la dirección IP del equipo remoto al que se va a conectar y, opcionalmente, el puerto TCP que se va a usar (el valor predeterminado es el puerto TCP 23). |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar Telnet para conectarse al equipo que ejecuta el servicio del servidor Telnet en *telnet.Microsoft.com*, escriba:

```
telnet telnet.microsoft.com
```

Para usar Telnet para conectarse al equipo que ejecuta el servicio del servidor Telnet en *telnet.Microsoft.com en el puerto TCP 44* y ro registrar la actividad de la sesión en un archivo local denominado *telnetlog.txt*, escriba:

```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Instalación de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10))

- [Referencia técnica de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754987(v=ws.10))
