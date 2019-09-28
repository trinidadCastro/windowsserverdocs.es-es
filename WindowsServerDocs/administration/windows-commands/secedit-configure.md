---
title: 'secedit: configurar'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0e1b900d01ad7f0e84d3235f24a00fe108eaa36
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384273"
---
# <a name="seceditconfigure"></a>secedit: configurar



Permite configurar las opciones actuales del sistema mediante la configuración de seguridad almacenada en una base de datos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|bases|Obligatorio.</br>Especifica la ruta de acceso y el nombre de un archivo de base de datos que contiene la configuración almacenada.</br>Si nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) `/cfg \<configuration file name>` asociada, también se debe especificar la opción de línea de comandos.|
|cfg|Opcional.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis.</br>Esta opción/cfg solo es válida cuando se usa con `/db \<database file name>` el parámetro. Si no se especifica, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad del parámetro/cfg debe sobrescribir cualquier plantilla o plantilla compuesta almacenada en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos solo es válida cuando `/cfg \<configuration file name>` se usa también el parámetro. Si no se especifica, la plantilla del parámetro/cfg se anexa a la plantilla almacenada.|
|Lugares|Opcional.</br>Especifica las áreas de seguridad que se van a aplicar al sistema. Si no se especifica este parámetro, se aplicará al sistema toda la configuración de seguridad definida en la base de datos. Para configurar varias áreas, separe cada área por un espacio. Se admiten las siguientes áreas de seguridad:</br>-SecurityPolicy</br>    Directiva local y Directiva de dominio para el sistema, incluidas las directivas de cuenta, las directivas de auditoría, las opciones de seguridad, etc.</br>- Group_Mgmt</br>    Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</br>- User_Rights</br>    Derechos de inicio de sesión de usuario y concesión de privilegios.</br>- RegKeys</br>    Seguridad de las claves del registro local.</br>-Almacén de.</br>    Seguridad en el almacenamiento de archivos local.</br>-Servicios</br>    Seguridad para todos los servicios definidos.|
|Inicia|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|Actividad|Opcional.</br>Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Si no se proporciona la ruta de acceso del archivo de registro, se usa el archivode registro \*predeterminado, (SystemRoot \Users cuentadeusuario<em>\*\Mis Documents\Security\Logs DatabaseName</em>. log).

A partir de Windows Server 2008 `Secedit /refreshpolicy` , se ha reemplazado por. `gpupdate` Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Example

Realice el análisis de los parámetros de seguridad en la base de datos de seguridad, SecDbContoso. sdb, que creó con el complemento configuración y análisis de seguridad. Dirija la salida al archivo SecAnalysisContosoFY11 con la solicitud para que pueda comprobar que el comando se ejecutó correctamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Supongamos que el análisis ha revelado algunos inadequacies, por lo que se ha modificado la plantilla de seguridad, SecContoso. inf. Vuelva a ejecutar el comando para incorporar los cambios y dirigir la salida al archivo SecAnalysisContosoFY11 existente sin preguntar.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)