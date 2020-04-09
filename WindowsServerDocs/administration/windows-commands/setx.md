---
title: setx
description: Temas de comandos de Windows para setx, que crea o modifica variables de entorno en el entorno de usuario o del sistema, sin necesidad de programación o scripting.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19f6625ffaaf745ae2af26e52986e97382f42702
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834348"
---
# <a name="setx"></a>setx

Crea o modifica variables de entorno en el entorno de usuario o del sistema, sin necesidad de programación o scripting. El comando **setx** también recupera los valores de las claves del registro y los escribe en archivos de texto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> <String>} [/m] | /x} [/d <Delimiters>]
```

### <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                                                              Descripción                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<equipo >       |                                                                                  Especifica el nombre o la dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el nombre del equipo local.                                                                                  |
| /u [\<\]de > de dominio <User name> |                                                                                           Ejecuta el script con las credenciales de la cuenta de usuario especificada. El valor predeterminado es los permisos del sistema.                                                                                            |
|      /p [\<contraseña >]      |                                                                                                         Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                         |
|        \<variable >         |                                                                                                                 Especifica el nombre de la variable de entorno que desea establecer.                                                                                                                  |
|          \<valor >          |                                                                                                                Especifica el valor en el que desea establecer la variable de entorno.                                                                                                                 |
|         /k \<ruta de acceso >         | Especifica que la variable se establece basándose en la información de una clave del registro. La*gistro* p usa la siguiente sintaxis:</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Por ejemplo, puede especificar la ruta de acceso siguiente:</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<nombre de archivo >       |                                                                                                                               Especifica el archivo que desea usar.                                                                                                                                |
|        /a \<X >,<Y>         |                                                                                                                    Especifica las coordenadas absolutas y el desplazamiento como parámetros de búsqueda.                                                                                                                    |
|   /r \<X >,<Y> <String>   |                                                                                                            Especifica las coordenadas relativas y el desplazamiento de la **cadena** como parámetros de búsqueda.                                                                                                            |
|             /m             |                                                                                                Especifica que se establezca la variable en el entorno del sistema. La configuración predeterminada es el entorno local.                                                                                                 |
|             /x             |                                                                                                       Muestra las coordenadas del archivo, pasando por alto las opciones de la línea de comandos **/a**, **/r**y **/d** .                                                                                                        |
|      /d \<delimitadores >      |                    Especifica los delimitadores como **,** o **\\** que se van a usar además de los cuatro delimitadores integrados: espacio, tabulación, entrada y avance de velocidad. Entre los delimitadores válidos se incluyen los caracteres ASCII. El número máximo de delimitadores es 15, incluidos los delimitadores integrados.                    |
|             /?             |                                                                                                                                 Muestra la Ayuda en el símbolo del sistema.                                                                                                                                  |

## <a name="remarks"></a>Comentarios

-   El comando **setx** es similar a la utilidad de UNIX setenv.
-   **Setx** proporciona la única línea de comandos o la manera programática de establecer de forma directa y permanente valores de entorno del sistema. Las variables de entorno del sistema se pueden configurar manualmente mediante el **Panel de control** o mediante un editor del registro. El comando **set** , que es interno del intérprete de comandos (cmd. exe), solo establece las variables de entorno de usuario para la ventana de la consola actual.
-   Puede usar el comando **setx** para establecer los valores de las variables de entorno de usuario y de sistema de uno de los tres orígenes (modos): modo de línea de comandos, modo de registro o modo de archivo.
-   **Setx** escribe variables en el entorno principal del registro. Las variables establecidas con variables **setx** solo están disponibles en las ventanas de comandos futuras, no en la ventana de comandos actual.
-   **HKEY_CURRENT_USER** y **HKEY_LOCAL_MACHINE** son los únicos subárboles admitidos. REG_DWORD, REG_EXPAND_SZ, REG_SZ y REG_MULTI_SZ son los tipos de datos **RegKey** válidos.
-   Cuando obtiene acceso a **REG_MULTI_SZ** valores del registro, solo se extrae y se usa el primer elemento.
-   No se puede usar el comando **setx** para quitar los valores que se han agregado a los entornos local o del sistema. Puede usar **set** con un nombre de variable y ningún valor para quitar un valor correspondiente del entorno local.
-   REG_DWORD valores del registro se extraen y se usan en modo hexadecimal.
-   El modo de archivo solo admite el análisis de archivos de texto de retorno de carro y avance de línea (CRLF).

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para establecer la variable de entorno del equipo en el entorno local en el valor Brand1, escriba:
```
setx MACHINE Brand1
```
Para establecer la variable de entorno del equipo en el entorno del sistema en el valor Brand1 equipo, escriba:
```
setx MACHINE Brand1 Computer /m
```
Para establecer la variable de entorno MYPATH en el entorno local para que use la ruta de acceso de búsqueda definida en la variable de entorno PATH, escriba:
```
setx MYPATH %PATH%
```
Para establecer la variable de entorno MYPATH en el entorno local para que use la ruta de acceso de búsqueda definida en la variable de entorno PATH después de reemplazar **~** por **%** , escriba:
```
setx MYPATH ~PATH~ 
```
Para establecer la variable de entorno del equipo en el entorno local en Brand1 en un equipo remoto denominado Equipo1, escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Para establecer la variable de entorno MYPATH en el entorno local para que use la ruta de acceso de búsqueda definida en la variable de entorno PATH en un equipo remoto denominado Equipo1, escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Para establecer la variable de entorno TZONE en el entorno local en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , escriba:
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Para establecer la variable de entorno TZONE en el entorno local de un equipo remoto llamado Equipo1 en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Para establecer la variable de entorno de compilación en el entorno del sistema en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , escriba:
```
setx BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber /m
```
Para establecer la variable de entorno de compilación en el entorno del sistema de un equipo remoto denominado Equipo1 en el valor que se encuentra en la clave del registro **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber /m
```
Para mostrar el contenido de un archivo denominado ipconfig. out, junto con las coordenadas correspondientes del contenido, escriba:
```
setx /f ipconfig.out /x
```
Para establecer la variable de entorno DIRIP en el entorno local en el valor que se encuentra en la coordenada 5, 11 del archivo ipconfig. out, escriba:
```
setx IPADDR /f ipconfig.out /a 5,11
```
Para establecer la variable de entorno OCTET1 en el entorno local en el valor que se encuentra en la coordenada 5, 3 en el archivo ipconfig. out con delimitadores **#$\*.** , escriba:
```
setx OCTET1 /f ipconfig.out /a 5,3 /d #$*. 
```
Para establecer la variable de entorno IPGATEWAY en el entorno local en el valor que se encuentra en la coordenada 0, 7 con respecto a la coordenada de la puerta de enlace en el archivo ipconfig. out, escriba:
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Para mostrar el contenido de un archivo denominado ipconfig. out, junto con las coordenadas correspondientes del contenido, en un equipo denominado Equipo1, escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)