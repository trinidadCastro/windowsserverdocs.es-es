---
title: gpresult
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c5d96e7e1fcaeadbbc6b67e1c816a8810fd0e3b6
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811185"
---
# <a name="gpresult"></a>gpresult

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la información del conjunto resultante de directivas (RSoP) para un usuario remoto y el equipo.
Para usar informes de RSoP para equipos dirigidos remotamente a través del firewall, debe tener reglas de firewall que permitan el tráfico de red entrante en los puertos.

## <a name="syntax"></a>Sintaxis

```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```

## <a name="parameters"></a>Parámetros

> [!NOTE]
> Excepto cuando se utiliza **/?** , debe incluir una opción de salida, ya sea **/r**, **/v**, **/z**, **/x**, o **/h**.

|                Parámetro                 |                                                                                                     Descripción                                                                                                      |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /s \<system\>               |                                                  Especifica el nombre o dirección IP de un equipo remoto. No use barras diagonales inversas. El valor predeterminado es el equipo local.                                                   |
|             /u \<nombre de usuario\>              |                                Usa las credenciales del usuario especificado para ejecutar el comando. El usuario predeterminado es el usuario que ha iniciado sesión en el equipo que ejecuta el comando.                                 |
|            /p [\<PASSWOrd\>]             |            Especifica la contraseña de la cuenta de usuario que se proporciona en el **/u** parámetro. Si **/p** se omite, **gpresult** pide la contraseña. **/p** no se puede usar con **/x** o **/h**.            |
| /User [\<TARGETDOMAIN\>\\]\<TARGETUSER\> |                                                                            Especifica el usuario remoto cuyos datos RSoP están que se mostrará.                                                                             |
|      el ámbito / {usuario &#124; equipo}       |                                Muestra los datos de RSoP para el usuario o el equipo. Si **ámbito/** se omite, **gpresult** muestra los datos de RSoP para el usuario y el equipo.                                 |
|        [/x &#124; /h] <FILENAME>         | Guarda el informe en XML ( **/x**) o HTML ( **/h**) formato en la ubicación y con el nombre de archivo especificado por el *FILENAME* parámetro. No se puede usar con **/u**, **/p**, **/r**, **/v**, o **/z**. |
|                    /f                    |                                                           fuerza **gpresult** para sobrescribir el nombre de archivo que se especifica en el **/x** o **/h** opción.                                                           |
|                    /r                    |                                                                                             Muestra los datos de resumen de RSoP.                                                                                              |
|                    /v                    |                                                    Muestra información detallada de directivas. Esto incluye la configuración detallada que se aplicaron con una prioridad de 1.                                                    |
|                    /z                    |                                     Muestra toda la información disponible acerca de la directiva de grupo. Esto incluye la configuración detallada que se aplicaron con una prioridad de 1 y versiones posteriores.                                      |
|                    /?                    |                                                                                         Muestra la ayuda en el símbolo del sistema.                                                                                         |

## <a name="remarks"></a>Comentarios
- Directiva de grupo es la principal herramienta administrativa para definir y controlar cómo funcionan los programas, los recursos de red y el sistema operativo para los usuarios y equipos de una organización. En un entorno de Active Directory, directiva de grupo se aplica a los usuarios o equipos según su pertenencia a sitios, dominios o unidades organizativas.
- Porque puede aplicar la configuración de directiva que se superponen a cualquier usuario o equipo, la característica de directiva de grupo genera un conjunto resultante de la configuración de directiva cuando el usuario inicia sesión. **gpresult** muestra el conjunto resultante de la configuración de directiva que se aplicó en el equipo para el usuario especificado cuando el usuario inició sesión.
- Dado que **/v** y **/z** generan grandes cantidades de información, es útil redirigir la salida a un archivo de texto (por ejemplo, **gpresult/z > policy.txt**).
- El **gpresult** comando está disponible en Windows Server 2012, Windows Server 2008 R2, Server 2008 de Windows, Windows 8, Windows 7 y Windows Vista.
  ## <a name="examples"></a>Ejemplos
  En el ejemplo siguiente se recuperan datos de RSoP para el usuario remoto **nombreUsuarioDestino** del equipo **srvmain**y muestra los datos RSoP sobre solo el usuario. El comando se ejecuta con las credenciales del usuario **maindom\hiropln**, y <strong>p@ssW23</strong> se introduce como la contraseña para ese usuario.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
  ```
  
En el ejemplo siguiente se guarda toda la información disponible acerca de la directiva de grupo para el usuario remoto **nombreUsuarioDestino** del equipo **srvmain** a un archivo que se denomina **policy.txt**. No hay datos se incluyen acerca del equipo. El comando se ejecuta con las credenciales del usuario **maindom\hiropln**, y <strong>p@ssW23</strong> se introduce como la contraseña para ese usuario.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
  ```
  
El ejemplo siguiente muestra los datos de RSoP para el equipo **srvmain** y el usuario ha iniciado sesión. Datos se incluyen sobre el usuario y el equipo. El comando se ejecuta con las credenciales del usuario **maindom\hiropln**, y <strong>p@ssW23</strong> se introduce como la contraseña para ese usuario.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
  ```
  
## <a name="additional-references"></a>Referencias adicionales
- [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
