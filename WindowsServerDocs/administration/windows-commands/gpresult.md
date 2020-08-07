---
title: gpresult
description: Artículo de referencia del comando Gpresult, que muestra la información del conjunto resultante de directivas (RSoP) para un usuario y un equipo remotos.
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c65dd4799441dca44db24f532be66349b1249a5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888579"
---
# <a name="gpresult"></a>gpresult

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra la información del conjunto resultante de directivas (RSoP) para un usuario y un equipo remotos. Para usar informes de RSoP para equipos de destino remotos a través del firewall, debe tener reglas de firewall que permitan el tráfico de red de entrada en los puertos.

## <a name="syntax"></a>Sintaxis

```
gpresult [/s <system> [/u <username> [/p [<password>]]]] [/user [<targetdomain>\]<targetuser>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <filename> [/f] | /?}
```

> [!NOTE]
> Excepto cuando se **usa/?**, debe incluir una opción de salida, **/r**, **/v**, **/z**, **/x**o **/h**.

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| modificado`<system>` | Especifica el nombre o la dirección IP de un equipo remoto. No use barras diagonales inversas. La opción predeterminada es el equipo local. |
| /u`<username>` | Usa las credenciales del usuario especificado para ejecutar el comando. El usuario predeterminado es el usuario que ha iniciado sesión en el equipo que emite el comando. |
| /p`[<password>]` | Especifica la contraseña de la cuenta de usuario que se proporciona en el parámetro **/u** . Si se omite **/p** , **Gpresult** solicita la contraseña. El parámetro **/p** no se puede usar con **/x** o **/h**. |
| /User`[<targetdomain>\]<targetuser>]` | Especifica el usuario remoto cuyos datos de RSoP se van a mostrar. |
| /Scope`{user | computer}` | Muestra datos RSoP para el usuario o el equipo. Si se omite **/Scope** , **Gpresult** muestra los datos de RSoP para el usuario y el equipo. |
| `[/x | /h] <filename>` | Guarda el informe en formato XML (**/x**) o HTML (**/h**) en la ubicación y con el nombre de archivo que se especifica mediante el parámetro *filename* . No se puede usar con **/u**, **/p**, **/r**, **/v**o **/z**. |
| /f | Fuerza a **Gpresult** a sobrescribir el nombre de archivo que se especifica en la opción **/x** o **/h** . |
| /r | Muestra datos de Resumen de RSoP. |
| /v | Muestra información detallada de la Directiva. Esto incluye la configuración detallada que se aplicó con una prioridad de 1. |
| /z | Muestra toda la información disponible sobre directiva de grupo. Esto incluye la configuración detallada que se aplicó con una prioridad de 1 y superior. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Directiva de grupo es la herramienta administrativa principal para definir y controlar el funcionamiento de los programas, los recursos de red y el sistema operativo para los usuarios y equipos de una organización. En un entorno de Active Directory, directiva de grupo se aplica a usuarios o equipos en función de su pertenencia a sitios, dominios o unidades organizativas.

- Dado que puede aplicar la configuración de directivas superpuestas a cualquier equipo o usuario, la característica directiva de grupo genera un conjunto de valores de configuración de directiva resultante cuando el usuario inicia sesión. El comando **Gpresult** muestra el conjunto resultante de opciones de configuración de directivas que se aplicaron en el equipo para el usuario especificado cuando el usuario inició sesión.

- Dado que **/v** y **/z** generan mucha información, resulta útil redirigir la salida a un archivo de texto (por ejemplo, `gpresult/z >policy.txt` ).

### <a name="examples"></a>Ejemplos

Para recuperar datos RSoP solo para el usuario remoto, *maindom\hiropln* con la contraseña *p@ssW23* , que se encuentra en el equipo *srvmain*, escriba:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```

Para guardar toda la información disponible sobre directiva de grupo en un archivo denominado *policy.txt*, solo para el usuario remoto *maindom\hiropln* con la contraseña *p@ssW23* , en el equipo *srvmain*, escriba:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```

Para mostrar los datos de RSoP del usuario que ha iniciado sesión, *maindom\hiropln* con la contraseña *p@ssW23* para el equipo *srvmain*, escriba:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
