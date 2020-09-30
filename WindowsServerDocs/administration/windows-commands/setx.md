---
title: setx
description: Artículo de referencia para el comando setx, que crea o modifica variables de entorno en el entorno de usuario o del sistema, sin necesidad de programación o scripting.
ms.topic: reference
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 417a00dbb037c298784df81f9c5ed5e9c28485f8
ms.sourcegitcommit: 881a5bd40026288afbcee5fdbf602fd55f833d47
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586445"
---
# <a name="setx"></a>setx

Crea o modifica variables de entorno en el entorno de usuario o del sistema, sin necesidad de programación o scripting. El comando **setx** también recupera los valores de las claves del registro y los escribe en archivos de texto.

> [!NOTE]
> Este comando proporciona la única línea de comandos o la manera programática de establecer de forma directa y permanente los valores de entorno del sistema. Las variables de entorno del sistema se pueden configurar manualmente mediante el **Panel de control** o mediante un editor del registro. El comando **set** , que es interno del intérprete de comandos (Cmd.exe), solo establece las variables de entorno de usuario para la ventana de la consola actual.

## <a name="syntax"></a>Sintaxis

```
setx [/s <computer> [/u [<domain>\]<user name> [/p [<password>]]]] <variable> <value> [/m]
setx [/s <computer> [/u [<domain>\]<user name> [/p [<password>]]]] <variable>] /k <path> [/m]
setx [/s <computer> [/u [<domain>\]<user name> [/p [<password>]]]] /f <filename> {[<variable>] {/a <X>,<Y> | /r <X>,<Y> <String>} [/m] | /x} [/d <delimiters>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el nombre del equipo local. |
| /u `[<domain>\]<user name>` | Ejecuta el script con las credenciales de la cuenta de usuario especificada. El valor predeterminado es los permisos del sistema. |
| /p [ `<password>` ]| Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `<variable>` | Especifica el nombre de la variable de entorno que desea establecer. |
| `<value>` | Especifica el valor en el que desea establecer la variable de entorno. |
| /k `<path>` | Especifica que la variable se establece basándose en la información de una clave del registro. La *ruta de acceso* utiliza la sintaxis siguiente: `\\<HIVE>\<KEY>\...\<Value>` . Por ejemplo, puede especificar la ruta de acceso siguiente: `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
| /f `<filename>` | Especifica el archivo que desea usar. |
| /a `<X>,<Y>` | Especifica las coordenadas absolutas y el desplazamiento como parámetros de búsqueda. |
| /r `<X>,<Y> <String>` | Especifica las coordenadas relativas y el desplazamiento de la **cadena** como parámetros de búsqueda. |
| /m | Especifica que se establezca la variable en el entorno del sistema. La configuración predeterminada es el entorno local. |
| /x | Muestra las coordenadas del archivo, pasando por alto las opciones de la línea de comandos **/a**, **/r**y **/d** . |
| /d. `<delimiters>` | Especifica los delimitadores como **,** o **\\** que se van a usar además de los cuatro delimitadores integrados: espacio, tabulación, entrada y avance de barra. Entre los delimitadores válidos se incluyen los caracteres ASCII. El número máximo de delimitadores es 15, incluidos los delimitadores integrados. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Este comando es similar a la utilidad de UNIX SETENV.

- Puede usar este comando para establecer los valores de las variables de entorno de usuario y de sistema de uno de los tres orígenes (modos): modo de línea de comandos, modo de registro o modo de archivo.

- Este comando escribe variables en el entorno principal del registro. Las variables establecidas con variables **setx** solo están disponibles en las ventanas de comandos futuras, no en la ventana de comandos actual.

- **HKEY_CURRENT_USER** y **HKEY_LOCAL_MACHINE** son los únicos subárboles admitidos. REG_DWORD, REG_EXPAND_SZ, REG_SZ y REG_MULTI_SZ son los tipos de datos **RegKey** válidos.

- Si obtiene acceso a **REG_MULTI_SZ** valores del registro, solo se extrae y se usa el primer elemento.

- No puede utilizar este comando para quitar los valores agregados a los entornos locales o del sistema. Puede usar este comando con un nombre de variable y sin valor para quitar un valor correspondiente del entorno local.

- REG_DWORD valores del registro se extraen y se usan en modo hexadecimal.

- El modo de archivo solo admite el análisis de archivos de texto de retorno de carro y avance de línea (CRLF).

- Al ejecutar este comando en una variable existente, se quitan las referencias a variables y se usan los valores expandidos.

  Por ejemplo, si la variable% PATH% tiene una referencia a% JAVADIR% y% PATH% se manipula mediante **setx**, se expande% JAVADIR% y su valor se asigna directamente a la variable de destino% path%. Esto significa que las actualizaciones futuras de% JAVADIR% **no** se reflejarán en la variable% PATH%.

- Tenga en cuenta que hay un límite de 1024 caracteres al asignar contenido a una variable mediante **setx**.

  Esto significa que el contenido se recorta si pasa por encima de 1024 caracteres y que el texto recortado es lo que se aplica a la variable de destino. Si este texto recortado se aplica a una variable existente, puede provocar la pérdida de datos que la variable de destino contenía anteriormente.

## <a name="examples"></a>Ejemplos

Para establecer la variable de entorno del *equipo* en el entorno local en el valor *Brand1*, escriba:

```
setx MACHINE Brand1
```

Para establecer la variable de entorno del *equipo* en el entorno del sistema en el valor *Brand1 equipo*, escriba:

```
setx MACHINE Brand1 Computer /m
```

Para establecer la variable de entorno *myPath* en el entorno local para que use la ruta de acceso de búsqueda definida en la variable de entorno *path* , escriba:

```
setx MYPATH %PATH%
```

Para establecer la variable *de entorno MYPATH* en el entorno local para que use la ruta de acceso de búsqueda definida en la variable de entorno *path* después de reemplazar **~** por **%** , escriba:

```
setx MYPATH ~PATH~
```

Para establecer la variable de entorno del *equipo* en el entorno local en *Brand1* en un equipo remoto denominado *Equipo1*, escriba:

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```

Para establecer la variable *de entorno MYPATH* en el entorno local para que use la ruta de acceso de búsqueda definida en la variable de entorno *path* en un equipo remoto denominado *Equipo1*, escriba:

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```

Para establecer la variable de entorno *Tzone* en el entorno local en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , escriba:

```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName
```

Para establecer la variable de entorno *Tzone* en el entorno local de un equipo remoto llamado *Equipo1* en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , escriba:

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName
```

Para establecer la variable de entorno de *compilación* en el entorno del sistema en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , escriba:

```
setx BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber /m
```

Para establecer la variable de entorno de compilación en el entorno del sistema de un equipo remoto denominado Equipo1 en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , escriba:

```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber /m
```

Para mostrar el contenido de un archivo denominado *ipconfig. out*, junto con las coordenadas correspondientes del contenido, escriba:

```
setx /f ipconfig.out /x
```

Para establecer la variable de entorno *DirIP* en el entorno local en el valor que se encuentra en la coordenada *5, 11* del archivo *ipconfig. out* , escriba:

```
setx IPADDR /f ipconfig.out /a 5,11
```

Para establecer la variable de entorno *OCTET1* en el entorno local en el valor que se encuentra en la coordenada *5, 3* en el archivo *ipconfig. out* con delimitadores **# $ *.**, escriba:

```
setx OCTET1 /f ipconfig.out /a 5,3 /d #$*.
```

Para establecer la variable de entorno *IPGATEWAY* en el entorno local en el valor que se encuentra en la coordenada *0, 7* con respecto a la coordenada de la *puerta de enlace* en el archivo *ipconfig. out* , escriba:

```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway
```

Para mostrar el contenido del archivo *ipconfig. out* , junto con las coordenadas correspondientes del contenido, en un equipo denominado *Equipo1*, escriba:

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
