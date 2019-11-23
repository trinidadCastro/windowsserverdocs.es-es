---
title: gpresult
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb61911450ea8c0c68af0cf1a35c2f571810504b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375659"
---
# <a name="gpresult"></a>gpresult

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la información del conjunto resultante de directivas (RSoP) para un usuario y un equipo remotos.
Para usar informes de RSoP para equipos de destino remotos a través del firewall, debe tener reglas de firewall que permitan el tráfico de red de entrada en los puertos.

## <a name="syntax"></a>Sintaxis

```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```

## <a name="parameters"></a>Parámetros

> [!NOTE]
> Excepto cuando use **/?** , debe incluir una opción de salida, ya sea **/r**, **/v**, **/z**, **/x**o **/h**.

|                Parámetro                 |                                                                                                     Descripción                                                                                                      |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /s \<System\>               |                                                  Especifica el nombre o la dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el equipo local.                                                   |
|             /u \<nombredeusuario\>              |                                Usa las credenciales del usuario especificado para ejecutar el comando. El usuario predeterminado es el usuario que ha iniciado sesión en el equipo que emite el comando.                                 |
|            /p [\<contraseña\>]             |            Especifica la contraseña de la cuenta de usuario que se proporciona en el parámetro **/u** . Si se omite **/p** , **Gpresult** solicita la contraseña. **/p** no se puede usar con **/x** o **/h**.            |
| /User [\<TARGETDOMAIN\>\\]\<TARGETUSER\> |                                                                            Especifica el usuario remoto cuyos datos de RSoP se van a mostrar.                                                                             |
|      /Scope {equipo &#124; de usuario}       |                                Muestra datos RSoP para el usuario o el equipo. Si se omite **/Scope** , **Gpresult** muestra los datos de RSoP para el usuario y el equipo.                                 |
|        [/x &#124; /h] <FILENAME>         | Guarda el informe en formato XML ( **/x**) o HTML ( **/h**) en la ubicación y con el nombre de archivo que se especifica mediante el parámetro *filename* . No se puede usar con **/u**, **/p**, **/r**, **/v**o **/z**. |
|                    /f                    |                                                           fuerza a **Gpresult** a sobrescribir el nombre de archivo que se especifica en la opción **/x** o **/h** .                                                           |
|                    /r                    |                                                                                             Muestra datos de Resumen de RSoP.                                                                                              |
|                    /v                    |                                                    Muestra información detallada de la Directiva. Esto incluye la configuración detallada que se aplicó con una prioridad de 1.                                                    |
|                    /z                    |                                     Muestra toda la información disponible sobre directiva de grupo. Esto incluye la configuración detallada que se aplicó con una prioridad de 1 y superior.                                      |
|                    /?                    |                                                                                         Muestra la ayuda en el símbolo del sistema.                                                                                         |

## <a name="remarks"></a>Observaciones
- Directiva de grupo es la herramienta administrativa principal para definir y controlar el funcionamiento de los programas, los recursos de red y el sistema operativo para los usuarios y equipos de una organización. En un entorno de Active Directory, directiva de grupo se aplica a usuarios o equipos en función de su pertenencia a sitios, dominios o unidades organizativas.
- Dado que puede aplicar la configuración de directivas superpuestas a cualquier equipo o usuario, la característica directiva de grupo genera un conjunto de valores de configuración de directiva resultante cuando el usuario inicia sesión. **Gpresult** muestra el conjunto resultante de opciones de configuración de directivas que se aplicaron en el equipo para el usuario especificado cuando el usuario inició sesión.
- Dado que **/v** y **/z** generan mucha información, resulta útil redirigir la salida a un archivo de texto (por ejemplo, **gpresult/z > Policy. txt**).
- El comando **Gpresult** está disponible en windows Server 2012, windows Server 2008 R2, Windows Server2008, Windows 8, Windows 7 y Windows Vista.
  ## <a name="examples"></a>Ejemplos
  En el ejemplo siguiente se recuperan los datos de RSoP para el usuario remoto **targetusername** del equipo **srvmain**y se muestran los datos RSoP sobre el usuario únicamente. El comando se ejecuta con las credenciales del usuario **maindom\hiropln**y se escribe <strong>p@ssW23</strong> como la contraseña para ese usuario.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
  ```
  
En el ejemplo siguiente se guarda toda la información disponible sobre directiva de grupo para el **targetusername** de usuario remoto del equipo **srvmain** en un archivo denominado **Policy. txt**. No se incluyen datos sobre el equipo. El comando se ejecuta con las credenciales del usuario **maindom\hiropln**y se escribe <strong>p@ssW23</strong> como la contraseña para ese usuario.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
  ```
  
En el ejemplo siguiente se muestran los datos de RSoP para el equipo **srvmain** y el usuario que ha iniciado sesión. Los datos se incluyen tanto para el usuario como para el equipo. El comando se ejecuta con las credenciales del usuario **maindom\hiropln**y se escribe <strong>p@ssW23</strong> como la contraseña para ese usuario.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
  ```
  
## <a name="additional-references"></a>referencias adicionales
- [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
