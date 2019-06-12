---
title: setx
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b2caceed6962bef22e7d546fa3b4469c9682b39
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441256"
---
# <a name="setx"></a>setx



Crea o modifica las variables de entorno en el usuario o el entorno del sistema, sin necesidad de programación o secuencias de comandos. El **Setx** comando también recupera los valores de las claves del registro y los escribe en archivos de texto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> "<String>"} [/m] | /x} [/d <Delimiters>]
```

## <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                                                              Descripción                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<equipo >       |                                                                                  Especifica el nombre o dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el nombre del equipo local.                                                                                  |
| /u [\<Domain>\]<User name> |                                                                                           Ejecuta el script con las credenciales de cuenta de usuario especificada. El valor predeterminado es que los permisos del sistema.                                                                                            |
|      /p [\<Password>]      |                                                                                                         Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                                                         |
|        \<Variable>         |                                                                                                                 Especifica el nombre de la variable de entorno que desea establecer.                                                                                                                  |
|          \<valor >          |                                                                                                                Especifica el valor al que desea establecer la variable de entorno.                                                                                                                 |
|         /k \<ruta de acceso >         | Especifica que la variable se establece basándose en información procedente de una clave del registro. Las p*ath* utiliza la sintaxis siguiente:</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Por ejemplo, podría especificar la ruta de acceso siguiente:</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<nombre de archivo >       |                                                                                                                               Especifica el archivo que desea usar.                                                                                                                                |
|        /a \<X>,<Y>         |                                                                                                                    Especifica las coordenadas absolutas y el desplazamiento como parámetros de búsqueda.                                                                                                                    |
|   /r \<X>,<Y> "<String>"   |                                                                                                            Especifica las coordenadas relativas y el desplazamiento de **cadena** como parámetros de búsqueda.                                                                                                            |
|             /m             |                                                                                                Especifica que se establezca la variable en el entorno del sistema. El valor predeterminado es el entorno local.                                                                                                 |
|             /x             |                                                                                                       Muestra las coordenadas, omitiendo archivo la **/a**, **/r**, y **/d** opciones de línea de comandos.                                                                                                        |
|      /d \<delimitadores >      |                    Especifica los delimitadores, como " **,** "o" **\\** " que se utilizará además de los cuatro delimitadores integrados: espacio, tabulación, ENTRAR y avance de línea. Los delimitadores válidos incluyen cualquier carácter ASCII. El número máximo de delimitadores es 15, incluidos los delimitadores integrados.                    |
|             /?             |                                                                                                                                 Muestra la ayuda en el símbolo del sistema.                                                                                                                                  |

## <a name="remarks"></a>Comentarios

-   El **Setx** comando es similar a la utilidad SETENV de UNIX.
-   **SETX** proporciona el modo sólo de línea de comandos o mediante programación directamente y permanente configurar sistema los valores de entorno. Variables de entorno del sistema son configurables manualmente a través de **Panel de Control** o a través de un editor del registro. El **establecer** comando, que es interno al intérprete de comandos (Cmd.exe), Establece las variables de entorno de usuario para la ventana de consola actual únicamente.
-   Puede usar el **setx** comando para establecer las variables de entorno de los valores de usuario y del sistema desde uno de estos tres orígenes (modos): Modo de línea de comandos, el modo de registro o modo de archivo.
-   **SETX** escribe variables en el entorno principal en el registro. Establecen las variables con **setx** variables están disponibles en futuras ventanas de comandos únicamente, no en la ventana de comandos actual.
-   **HKEY_CURRENT_USER** y **HKEY_LOCAL_MACHINE** son las únicas secciones admitidas. REG_DWORD, REG_EXPAND_SZ, REG_SZ y REG_MULTI_SZ son válido **RegKey** tipos de datos.
-   Cuando se obtiene acceso a **REG_MULTI_SZ** se extrae y se usan los valores del registro, solo el primer elemento.
-   No puede usar el **setx** comando para quitar los valores que se han agregado a los entornos locales o del sistema. Puede usar **establecer** con un nombre de variable y ningún valor para quitar un valor correspondiente del entorno local.
-   Valores del registro REG_DWORD se extraen y se usa en modo hexadecimal.
-   Modo de archivo admite el análisis de retorno de carro y avance de línea solo los archivos de texto (CRLF).

## <a name="BKMK_examples"></a>Ejemplos

Para establecer la variable de entorno de máquina en el entorno local en el valor Brand1, escriba:
```
setx MACHINE Brand1
```
Para establecer la variable de entorno de máquina en el entorno del sistema en el valor Brand1 Computer, escriba:
```
setx MACHINE "Brand1 Computer" /m
```
Para establecer la variable de entorno MYPATH en el entorno local para usar la ruta de búsqueda definida en la variable de entorno PATH, escriba:
```
setx MYPATH %PATH%
```
Para establecer la variable de entorno MYPATH en el entorno local para usar la ruta de búsqueda definida en la variable de entorno PATH después de reemplazar **~** con **%** , tipo:
```
setx MYPATH ~PATH~ 
```
Para establecer la variable de entorno de máquina en el entorno local a Brand1 en un equipo remoto denominado Computer1, escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Para establecer la variable de entorno MYPATH en el entorno local para usar la ruta de búsqueda definida en la variable de entorno de ruta de acceso en un equipo remoto denominado Computer1, escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Para establecer la variable de entorno TZONE en el entorno local para el valor encontrado en el **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** clave, tipo de registro:
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Para establecer la variable de entorno TZONE en el entorno local de un equipo remoto denominado Computer1 en el valor situado en la **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** registro tipo de clave, de:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Para establecer la variable de entorno de compilación en el entorno del sistema para el valor encontrado en el **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber** clave, tipo de registro:
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
Para establecer la variable de entorno de compilación en el entorno de sistema de un equipo remoto denominado Computer1 en el valor situado en la **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber** clave del registro , escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
Para mostrar el contenido de un archivo denominado Ipconfig.out, junto con las coordenadas correspondiente de contenido, escriba:
```
setx /f ipconfig.out /x
```
Para establecer la variable de entorno IPADDR en el entorno local en el valor que se encuentra en la coordenada 5,11 en el archivo Ipconfig.out, escriba:
```
setx IPADDR /f ipconfig.out /a 5,11
```
Para establecer la variable de entorno OCTET1 en el entorno local en el valor se encuentra en la coordenada 5,3 en el archivo Ipconfig.out con delimitadores **"#$\*."** , tipo:
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
Para establecer la variable de entorno IPGATEWAY en el entorno local en el valor que se encuentra en la coordenada 0,7 con respecto a la coordenada de la "Puerta de enlace" en el archivo Ipconfig.out, escriba:
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Para mostrar el contenido de un archivo denominado Ipconfig.out, junto con coordenadas correspondientes el contenido, en un equipo denominado Computer1, escriba:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)