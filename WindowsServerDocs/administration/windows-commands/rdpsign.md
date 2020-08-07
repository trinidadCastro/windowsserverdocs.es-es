---
title: rdpsign
description: Artículo de referencia para el comando rdpsign, que permite firmar digitalmente un archivo Protocolo de escritorio remoto (. RDP).
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: f937cd0bebd2c57c4eef44c9dbf803176f285c2f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884328"
---
# <a name="rdpsign"></a>rdpsign

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite firmar digitalmente un archivo Protocolo de escritorio remoto (. RDP).

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /sha1`<hash>` | Especifica la huella digital, que es el hash Algoritmo hash seguro 1 (SHA1) del certificado de firma que se incluye en el almacén de certificados. Se usa en Windows Server 2012 R2 y versiones anteriores. |
| /sha256`<hash>` | Especifica la huella digital, que es el hash del algoritmo hash seguro 256 (SHA256) del certificado de firma que se incluye en el almacén de certificados. Reemplaza/SHA1 en Windows Server 2016 y versiones más recientes. |
| /q | Modo silencioso. No hay ninguna salida cuando el comando se ejecuta correctamente y la salida mínima si se produce un error en el comando. |
| /v | modo detallado. Muestra todas las advertencias, mensajes y estado. |
| /l | Comprueba los resultados de la firma y la salida sin reemplazar realmente ninguno de los archivos de entrada. |
| `<file_name.rdp>` | Nombre del archivo. RDP. Debe especificar el archivo. RDP (o archivos) para firmar con el nombre de archivo completo. No se aceptan caracteres comodín. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- La huella digital del certificado SHA1 o SHA256 debe representar un editor de archivos. RDP de confianza. Para obtener la huella digital del certificado, abra el complemento **certificados** , haga doble clic en el certificado que desea usar (ya sea en el almacén de certificados del equipo local o en el almacén de certificados personales), haga clic en la pestaña **detalles** y, a continuación, en la lista de **campos** , haga clic en **huella digital**.

    > [!NOTE]
    > Al copiar la huella digital para usarla con la herramienta rdpsign.exe, debe quitar los espacios.

- Los archivos de salida firmados sobrescriben los archivos de entrada.

- Si se especifican varios archivos y no se puede leer o escribir en alguno de los archivos. RDP, la herramienta continúa en el siguiente archivo.

### <a name="examples"></a>Ejemplos

Para firmar un archivo. RDP denominado *archivo1. RDP*, navegue hasta la carpeta donde guardó el archivo. RDP y, a continuación, escriba:

```
rdpsign /sha1 hash file1.rdp
```

> [!NOTE]
> El valor *hash* representa la huella digital del certificado SHA1, sin espacios en blanco.

Para comprobar si la firma digital se realizará correctamente en un archivo. RDP sin firmar realmente el archivo, escriba:

```
rdpsign /sha1 hash /l file1.rdp
```

Para firmar varios archivos. RDP denominados, *archivo1. RDP*, *archivo2. RDP*y *archivo3. RDP*, escriba (incluidos los espacios entre los nombres de archivo):

```
rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
```

## <a name="see-also"></a>Consulte también

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
