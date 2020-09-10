---
title: prndrvr
description: Artículo de referencia del comando prndrvr, que agrega, elimina y enumera los controladores de impresora.
ms.topic: reference
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6fd276adb02281ab488c31db75563552f8495008
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638353"
---
# <a name="prndrvr"></a>prndrvr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega, elimina y enumera los controladores de impresora. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prndrvr o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr`.

Si se usa sin parámetros, **prndrvr** muestra la ayuda de la línea de comandos.

## <a name="syntax"></a>Sintaxis

```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] [-e <environment>] [-s <Servername>] [-u <Username>] [-w <password>] [-h <path>] [-i <inf file>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -a | Instala un controlador. |
| -d | Elimina un controlador. |
| -l | Enumera todos los controladores de impresora instalados en el servidor especificado por el parámetro **-s** . Si no especifica un servidor, Windows enumera los controladores de impresora instalados en el equipo local. |
| -X | Elimina todos los controladores de impresora y controladores de impresora adicionales no utilizados por una impresora lógica en el servidor especificado por el parámetro **-s** . Si no especifica un servidor para quitar de la lista, Windows eliminará todos los controladores de impresora no usados en el equipo local. |
| -m `<model_name>` | Especifica (por nombre) el controlador que desea instalar. Los controladores a menudo se denominan para el modelo de impresora que admiten. Consulte la documentación de la impresora para obtener más información. |
| `-v {0|1|2|3}` | Especifica la versión del controlador que desea instalar. Vea la descripción del parámetro **-e**para obtener información sobre qué versiones están disponibles para cada entorno. Si no especifica una versión, se instala la versión del controlador adecuada para la versión de Windows que se ejecuta en el equipo en el que está instalando el controlador. |
| -e `<environment>` | Especifica el entorno para el controlador que desea instalar. Si no especifica un entorno, se usa el entorno del equipo en el que va a instalar el controlador. Los parámetros de entorno admitidos son: **Windows NT x86**, **Windows x64** o **Windows ia64**. |
| -s `<Servername>` | Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local. |
| -u `<Username>` -w `<password>` | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
| -h `<path>` | Especifica la ruta de acceso al archivo del controlador. Si no especifica una ruta de acceso, se usa la ruta de acceso a la ubicación en la que se instaló Windows. |
| -i `<filename.inf>` | Especifica la ruta de acceso completa y el nombre de archivo del controlador que desea instalar. Si no especifica un nombre de archivo, el script usa uno de los archivos Printer. inf de la bandeja de entrada en el subdirectorio inf del directorio de Windows.<p>Si no se especifica la ruta de acceso del controlador, el script busca archivos de controlador en el archivo de driver.cab. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

- El parámetro **-x** elimina todos los controladores de impresora adicionales (controladores instalados para su uso en clientes que ejecutan versiones alternativas de Windows), incluso si el controlador principal está en uso. Si el componente de fax está instalado, esta opción también elimina los controladores de fax. Si no está en uso, se elimina el controlador de fax principal (es decir, si no hay ninguna cola que lo use). Si se elimina el controlador de fax principal, la única manera de volver a habilitar fax es reinstalar el componente de fax.

### <a name="examples"></a>Ejemplos

Para enumerar todos los controladores en el servidor local de \\ servidorimpresión1, escriba:

```
cscript prndrvr -l -s
```

Para agregar un controlador de impresora de Windows x64 de la versión 3 para el modelo de impresora láser modelo 1 con el archivo de información del controlador c:\temp\Laserprinter1.inf para un controlador almacenado en la carpeta c:\temp, escriba:

```
cscript prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64 -i c:\temp\Laserprinter1.inf -h c:\temp
```

Para eliminar un controlador de impresora de Windows x64 de la versión 3 para la impresora láser modelo 1, escriba:

```
cscript prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
