---
title: ftp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3639626962e295a54c9068c9731f1d30754a9472
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438370"
---
# <a name="ftp"></a>ftp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfiere archivos a y desde un equipo que ejecuta un servicio de servidor de protocolo de transferencia de archivos (ftp). **FTP** puede usarse interactivamente o en modo por lotes mediante el procesamiento de archivos de texto ASCII. 
## <a name="syntax"></a>Sintaxis
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                                                                      Descripción                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Suprime la presentación de las respuestas del servidor remoto.                                                                                                                                     |
|        -d         |                                                                                                               Habilita la depuración, mostrando todos los comandos que se pasan entre el cliente FTP y el servidor FTP.                                                                                                                |
|        -i         |                                                                                                                            Deshabilita interactivas de datos durante las transferencias de archivos varios.                                                                                                                             |
|        -n         |                                                                                                                                    Suprime el inicio de sesión automático en la conexión inicial.                                                                                                                                     |
|        -g         |                                         Deshabilita el uso de comodines de nombre de archivo.  **Glob** permite usar el asterisco (\*) y signo de interrogación (?) como caracteres comodín en nombres de archivo y ruta de acceso locales. Para obtener más información, consulte [referencias adicionales](ftp.md#BKMK_additionalRef).                                          |
|   -s:<FileName>   | Especifica un archivo de texto que contiene **ftp** comandos. Estos comandos se ejecutan automáticamente después **ftp** se inicia. Este parámetro no permite espacios en blanco. Utilice este parámetro en lugar de redirección ( **<** ). **Nota:** En Windows 8 y Windows Server 2012 o sistemas operativos posteriores, se debe escribir el archivo de texto en UTF-8. |
|        -a         |                                                                                                                 Especifica que cualquier interfaz local se puede usar al enlazar la conexión de datos de ftp.                                                                                                                  |
|        -A         |                                                                                                                                        Inicie sesión en el servidor ftp anónimo.                                                                                                                                         |
|  -x:<SendBuffer>  |                                                                                                                                     Invalida el tamaño SO_SNDBUF predeterminado de 8192.                                                                                                                                     |
|  -r:<RecvBuffer>  |                                                                                                                                     Invalida el tamaño SO_RCVBUF predeterminado de 8192.                                                                                                                                     |
| -b:<AsyncBuffers> |                                                                                                                                    Invalida el recuento de búfer predeterminado asincrónico de 3.                                                                                                                                     |
| -w:<WindowsSize>  |                                                                                                                   Especifica el tamaño del búfer de transferencia. El tamaño de ventana predeterminado es 4096 bytes.                                                                                                                   |
|        -?         |                                                                                                                                         Muestra la ayuda en el símbolo del sistema.                                                                                                                                          |
|      <host>       |                                                                    Especifica el nombre del equipo, la dirección IP o la dirección IPv6 del servidor ftp que se va a conectar. El nombre de host o la dirección, si se especifica, debe ser el último parámetro en la línea.                                                                    |

## <a name="remarks"></a>Comentarios
- Para obtener más información acerca de **ftp** comandos en Windows Server 2003, consulte [ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- **FTP** parámetros de línea de comandos distinguen mayúsculas de minúsculas.
- Este comando está disponible solo si el **Protocolo Internet (TCP/IP)** está instalado como un componente en las propiedades de un adaptador de red en las conexiones de red.
- **FTP** puede utilizarse de forma interactiva. Una vez iniciado, **ftp** crea un entorno de subproceso en que se puede utilizar **ftp** comandos. Puede volver a la línea de comandos escribiendo el **salga** comando. Cuando el **ftp** se está ejecutando en el entorno secundario, se indica mediante el **ftp >** símbolo del sistema. Para obtener más información, consulte el **ftp** comandos.
- **FTP** admite el uso de IPv6 cuando se instala el protocolo IPv6. Para obtener más información, consulte [referencias adicionales](ftp.md#BKMK_additionalRef).
  ## <a name="BKMK_Examples"></a>Ejemplos
  Para iniciar sesión en el servidor ftp llamado FTP.ejemplo.Microsoft.com, escriba:
  ```
  ftp ftp.example.microsoft.com
  ```
  Para iniciar sesión en el servidor ftp llamado FTP.ejemplo.Microsoft.com y ejecutar el **ftp** los comandos contenidos en un archivo denominado resync.txt, tipo:
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>Referencias adicionales
- [IP versión 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Aplicaciones de IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
