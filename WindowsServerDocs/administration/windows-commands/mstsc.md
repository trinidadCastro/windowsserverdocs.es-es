---
title: mstsc
description: Artículo de referencia del comando mstsc, que crea conexiones a Escritorio remoto servidores host de sesión o a otros equipos remotos, edita un archivo de configuración existente de Conexión a Escritorio remoto (. RDP) y migra los archivos de conexión heredados que se crearon con el administrador de conexiones de cliente a los nuevos archivos de conexión. RDP.
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abf4824c50b931944c96307f6c9df42e6924b638
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886151"
---
# <a name="mstsc"></a>mstsc

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea conexiones para Escritorio remoto servidores host de sesión u otros equipos remotos, edita un archivo de configuración existente de Conexión a Escritorio remoto (. RDP) y migra los archivos de conexión heredados que se crearon con el administrador de conexiones de cliente a nuevos archivos de conexión. RDP.

## <a name="syntax"></a>Sintaxis

```
mstsc.exe [<connectionfile>] [/v:<server>[:<port>]] [/admin] [/f] [/w:<width> /h:<height>] [/public] [/span]
mstsc.exe /edit <connectionfile>
mstsc.exe /migrate
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ------------|
| `<connectionfile>` | Especifica el nombre de un archivo. RDP para la conexión. |
| /v:`<server>[:<port>]` | Especifica el equipo remoto y, opcionalmente, el número de puerto al que desea conectarse. |
| /admin | Le conecta a una sesión para administrar el servidor. |
| /f | Inicia Conexión a Escritorio remoto en modo de pantalla completa. |
| /w`<width>` | Especifica el ancho de la ventana de Escritorio remoto. |
| /h`<height>` | Especifica el alto de la ventana Conexión a Escritorio remoto. |
| /public | Ejecuta Escritorio remoto en modo público. En modo público, las contraseñas y los mapas de bits no se almacenan en caché. |
| /span | Coincide con el ancho y el alto de Escritorio remoto con el escritorio virtual local, lo que abarca varios monitores si es necesario. |
| /Edit`<connectionfile>` | Abre el archivo. RDP especificado para su edición. |
| /migrate | Migra los archivos de conexión heredados que se crearon con el administrador de conexiones de cliente a los nuevos archivos de conexión. RDP. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Default. RDP se almacena para cada usuario como un archivo oculto en la carpeta de **documentos** del usuario.

- Los archivos. RDP creados por el usuario se guardan de forma predeterminada en la carpeta de **documentos** del usuario, pero se pueden guardar en cualquier lugar.

- Para abarcar varios monitores, los monitores deben usar la misma resolución y deben estar alineados horizontalmente (es decir, en paralelo). Actualmente no se admite la expansión de varios monitores verticalmente en el sistema cliente.

### <a name="examples"></a>Ejemplos

Para conectarse a una sesión en modo de pantalla completa, escriba:

```
mstsc /f
```

Para abrir un archivo llamado *filename. RDP* para su edición, escriba:

```
mstsc /edit filename.rdp
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
