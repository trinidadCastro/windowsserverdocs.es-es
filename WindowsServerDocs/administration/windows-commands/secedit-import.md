---
title: 'secedit: importar'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47cd3b74a52af149706c6e6d238a076a9bc61f98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834888"
---
# <a name="seceditimport"></a>secedit: importar



Importa la configuración de seguridad almacenada en un archivo INF exportado previamente desde la base de datos configurada con plantillas de seguridad. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|bases|Obligatorio.</br>Especifica la ruta de acceso y el nombre de un archivo de base de datos que contiene la configuración almacenada en la que se realizará la importación.</br>Si nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) asociada, también se debe especificar la opción de línea de comandos `/cfg \<configuration file name>`.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad del parámetro/cfg debe sobrescribir cualquier plantilla o plantilla compuesta almacenada en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos solo es válida cuando se usa también el parámetro `/cfg \<configuration file name>`. Si no se especifica, la plantilla del parámetro/cfg se anexa a la plantilla almacenada.|
|cfg|Obligatorio.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis.</br>Esta opción/cfg solo es válida cuando se usa con el parámetro `/db \<database file name>`. Si no se especifica, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad del parámetro/cfg debe sobrescribir cualquier plantilla o plantilla compuesta almacenada en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos solo es válida cuando se usa también el parámetro `/cfg \<configuration file name>`. Si no se especifica, la plantilla del parámetro/cfg se anexa a la plantilla almacenada.|
|áreas|Opcional.</br>Especifica las áreas de seguridad que se van a aplicar al sistema. Si no se especifica este parámetro, se aplicará al sistema toda la configuración de seguridad definida en la base de datos. Para configurar varias áreas, separe cada área por un espacio. Se admiten las siguientes áreas de seguridad:</br>-SecurityPolicy</br>    Directiva local y Directiva de dominio para el sistema, incluidas las directivas de cuenta, las directivas de auditoría, las opciones de seguridad, etc.</br>-Group_Mgmt</br>    Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</br>-User_Rights</br>    Derechos de inicio de sesión de usuario y concesión de privilegios.</br>- RegKeys</br>    Seguridad de las claves del registro local.</br>-Almacén de.</br>    Seguridad en el almacenamiento de archivos local.</br>-Servicios</br>    Seguridad para todos los servicios definidos.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|actividad|Opcional.</br>Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Antes de importar un archivo. inf en otro equipo, ejecute el comando secedit/generaterollback en la base de datos en la que se realizará la importación y en secedit/Validate en el archivo de importación para comprobar su integridad.

Si no se proporciona la ruta de acceso del archivo de registro, se usa el archivo de registro predeterminado, (*systemroot*\Documents and settings\*cuentadeusuario<em>\Mis Documents\Security\Logs\*DatabaseName</em>. log).

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Exporte la base de datos de seguridad y las directivas de seguridad de dominio a un archivo INF e importe el archivo a una base de datos diferente para replicar la configuración de la Directiva de seguridad en otro equipo.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importe solo la parte de directivas de seguridad del archivo a una base de datos diferente en otro equipo.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Referencias adicionales

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)