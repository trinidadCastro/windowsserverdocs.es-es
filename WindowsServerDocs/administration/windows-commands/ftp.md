---
title: ftp
description: Tema de referencia del comando FTP, que transfiere archivos hacia y desde un equipo que ejecuta un servicio de servidor File Transfer Protocol (FTP).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3920306ce05aeb1b1e364c8146c461ea187f6560
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820245"
---
# <a name="ftp"></a>ftp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Transfiere archivos a y desde un equipo que ejecuta un servicio de servidor de File Transfer Protocol (FTP). Este comando se puede usar de forma interactiva o en modo por lotes mediante el procesamiento de archivos de texto ASCII.

## <a name="syntax"></a>Sintaxis

```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<filename>] [-a] [-A] [-x:<sendbuffer>] [-r:<recvbuffer>] [-b:<asyncbuffers>][-w:<windowssize>][<host>] [-?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ----------| ----------- |
| -v | Suprime la presentación de las respuestas del servidor remoto. |
| -d | Habilita la depuración, que muestra todos los comandos que se pasan entre el cliente FTP y el servidor FTP. |
| -i | Deshabilita la solicitud interactiva durante varias transferencias de archivos. |
| -n | Suprime el inicio de sesión automático después de la conexión inicial. |
| -g | Deshabilita el nombre de archivo comodines.  **Glob** permite el uso del asterisco (*) y el signo de interrogación (?) como caracteres comodín en los nombres de archivo y ruta de acceso locales. |
| seg`<filename>` | Especifica un archivo de texto que contiene comandos **FTP** . Estos comandos se ejecutan automáticamente después de iniciar **FTP** . Este parámetro no permite ningún espacio. Utilice este parámetro en lugar de la redirección ( `<` ). **Nota:** En los sistemas operativos Windows 8 y Windows Server 2012 o posterior, el archivo de texto debe estar escrito en UTF-8. |
| -a | Especifica que se puede utilizar cualquier interfaz local al enlazar la conexión de datos FTP. |
| -A | Inicia sesión en el servidor FTP como anónimo. |
| x1`<sendbuffer> `| Invalida el tamaño de SO_SNDBUF predeterminado de 8192. |
| c`<recvbuffer>` | Invalida el tamaño de SO_RCVBUF predeterminado de 8192. |
| b`<asyncbuffers>` | Invalida el recuento de búferes asincrónicos predeterminado de 3. |
| con`<windowssize>` | Especifica el tamaño del búfer de transferencia. El tamaño predeterminado de la ventana es de 4096 bytes. |
| `<host>` | Especifica el nombre del equipo, la dirección IP o la dirección IPv6 del servidor FTP al que se va a conectar. El nombre o la dirección de host, si se especifica, debe ser el último parámetro de la línea. |
| -? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los parámetros de la línea de comandos de **FTP** distinguen mayúsculas de minúsculas.

- Este comando solo está disponible si el protocolo **Protocolo de Internet (TCP/IP)** se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

- El comando **FTP** se puede usar de forma interactiva. Una vez iniciado, **FTP** crea un subentorno en el que puede usar comandos **FTP** . Puede volver al símbolo del sistema escribiendo el comando **Quit** . Cuando se está ejecutando el subentorno **FTP** , se indica mediante el `ftp >` símbolo del sistema. Para obtener más información, consulte los comandos **FTP** .

- El comando **FTP** admite el uso de IPv6 cuando el protocolo IPv6 está instalado.

### <a name="examples"></a>Ejemplos

Para iniciar sesión en el servidor FTP denominado `ftp.example.microsoft.com` , escriba:

```
ftp ftp.example.microsoft.com
```

Para iniciar sesión en el servidor FTP denominado `ftp.example.microsoft.com` y ejecutar los comandos **FTP** contenidos en un archivo denominado *Resync. txt*, escriba:

```
ftp -s:resync.txt ftp.example.microsoft.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))

- [IP versión 6](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc738636(v=ws.10))

- [Aplicaciones IPv6](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc782509(v=ws.10))
