---
title: ftp
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b64e6831fcf8930c62b2a3f04022dc49d5297683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375858"
---
# <a name="ftp"></a>ftp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfiere archivos a y desde un equipo que ejecuta un servicio de servidor de File Transfer Protocol (FTP). **FTP** se puede usar de forma interactiva o en modo por lotes mediante el procesamiento de archivos de texto ASCII. 
## <a name="syntax"></a>Sintaxis
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                                                                      Descripción                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Suprime la presentación de las respuestas del servidor remoto.                                                                                                                                     |
|        -d         |                                                                                                               Habilita la depuración, que muestra todos los comandos que se pasan entre el cliente FTP y el servidor FTP.                                                                                                                |
|        -i         |                                                                                                                            Deshabilita la solicitud interactiva durante varias transferencias de archivos.                                                                                                                             |
|        -n         |                                                                                                                                    Suprime el inicio de sesión automático después de la conexión inicial.                                                                                                                                     |
|        -g         |                                         Deshabilita el nombre de archivo comodines.  **Glob** permite el uso del asterisco (\*) y el signo de interrogación (?) como caracteres comodín en los nombres de archivo y ruta de acceso locales. Para obtener más información, consulte [referencias adicionales](ftp.md#BKMK_additionalRef).                                          |
|   -s: <FileName>   | Especifica un archivo de texto que contiene comandos **FTP** . Estos comandos se ejecutan automáticamente después de iniciar **FTP** . Este parámetro no permite ningún espacio. Utilice este parámetro en lugar de la redirección ( **<** ). **Nota:** En los sistemas operativos Windows 8 y Windows Server 2012 o posterior, el archivo de texto debe estar escrito en UTF-8. |
|        -a         |                                                                                                                 Especifica que se puede utilizar cualquier interfaz local al enlazar la conexión de datos FTP.                                                                                                                  |
|        -A         |                                                                                                                                        Inicia sesión en el servidor FTP como anónimo.                                                                                                                                         |
|  -x: <SendBuffer>  |                                                                                                                                     Invalida el tamaño de SO_SNDBUF predeterminado de 8192.                                                                                                                                     |
|  -r: <RecvBuffer>  |                                                                                                                                     Invalida el tamaño de SO_RCVBUF predeterminado de 8192.                                                                                                                                     |
| -b: <AsyncBuffers> |                                                                                                                                    Invalida el recuento de búferes asincrónicos predeterminado de 3.                                                                                                                                     |
| -w: <WindowsSize>  |                                                                                                                   Especifica el tamaño del búfer de transferencia. El tamaño predeterminado de la ventana es de 4096 bytes.                                                                                                                   |
|        -?         |                                                                                                                                         Muestra la ayuda en el símbolo del sistema.                                                                                                                                          |
|      <host>       |                                                                    Especifica el nombre del equipo, la dirección IP o la dirección IPv6 del servidor FTP al que se va a conectar. El nombre o la dirección de host, si se especifica, debe ser el último parámetro de la línea.                                                                    |

## <a name="remarks"></a>Comentarios
- para obtener más información acerca de los comandos **FTP** en Windows Server 2003, consulte [FTP](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- los parámetros de la línea de comandos **FTP** distinguen mayúsculas de minúsculas.
- Este comando solo está disponible si el protocolo **Protocolo de Internet (TCP/IP)** se instala como componente en las propiedades de un adaptador de red en las conexiones de red.
- **FTP** se puede usar de forma interactiva. Una vez iniciado, **FTP** crea un subentorno en el que puede usar comandos **FTP** . Puede volver al símbolo del sistema escribiendo el comando **Quit** . Cuando se está ejecutando el subentorno **FTP** , se indica mediante el > del símbolo del sistema de **FTP** . Para obtener más información, consulte los comandos **FTP** .
- **FTP** admite el uso de IPv6 cuando se instala el protocolo IPv6. Para obtener más información, consulte [referencias adicionales](ftp.md#BKMK_additionalRef).
  ## <a name="BKMK_Examples"></a>Example
  Para iniciar sesión en el servidor FTP denominado ftp.example.microsoft.com, escriba:
  ```
  ftp ftp.example.microsoft.com
  ```
  Para iniciar sesión en el servidor FTP denominado ftp.example.microsoft.com y ejecutar los comandos **FTP** contenidos en un archivo denominado Resync. txt, escriba:
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>referencias adicionales
- [IP versión 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Aplicaciones IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
