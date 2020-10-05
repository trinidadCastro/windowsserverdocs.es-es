---
title: tftp
description: Artículo de referencia para el comando TFTP, que transfiere archivos hacia y desde un equipo remoto.
ms.topic: reference
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 89a4edb69753ae34814d13f90f21332c6f748244
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717912"
---
# <a name="tftp"></a>tftp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Transfiere archivos a y desde un equipo remoto, normalmente un equipo que ejecuta UNIX, que ejecuta el servicio o el demonio de File Transfer Protocol trivial (TFTP). TFTP suele usarse en dispositivos o sistemas incrustados que recuperan el firmware, la información de configuración o una imagen del sistema durante el proceso de arranque desde un servidor TFTP.

> AÚN El protocolo TFTP no admite ningún mecanismo de autenticación o cifrado y, por lo tanto, puede suponer un riesgo de seguridad cuando está presente. No se recomienda instalar el cliente TFTP para sistemas conectados a Internet. Microsoft ya no proporciona un servicio de servidor TFTP por motivos de seguridad.

## <a name="syntax"></a>Sintaxis

```
tftp [-i] [<host>] [{get | put}] <source> [<destination>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -i | Especifica el modo de transferencia de imagen binaria (también denominado modo de octeto). En el modo de imagen binaria, el archivo se transfiere en unidades de un byte. Utilice este modo al transferir archivos binarios. Si no usa la opción **-i** , el archivo se transfiere en modo ASCII. Este es el modo de transferencia predeterminado. Este modo convierte los caracteres de fin de línea (EOL) a un formato adecuado para el equipo especificado. Utilice este modo al transferir archivos de texto. Si una transferencia de archivos se realiza correctamente, se muestra la velocidad de transferencia de datos. |
| `<host>` | Especifica el equipo local o remoto. |
| get | Transfiere el *destino* de archivo del equipo remoto al *origen* de archivo en el equipo local. |
| put | Transfiere el *origen* de archivo del equipo local al *destino* del archivo en el equipo remoto. Dado que el protocolo TFTP no admite la autenticación de usuario, el usuario debe haber iniciado sesión en el equipo remoto y los archivos deben poder escribirse en el equipo remoto. |
| `<source>` | Especifica el archivo que se va a transferir. |
| `<destination>` | Especifica dónde se debe transferir el archivo. |

## <a name="examples"></a>Ejemplos

Para copiar el archivo *boot. img* desde el equipo remoto *Host1*, escriba:

```
tftp  -i Host1 get boot.img
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
