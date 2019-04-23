---
title: secedit:import
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f24cf173d1bacd70d92b325bfe7b342d0589a490
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874286"
---
# <a name="seceditimport"></a>secedit:import



Importa la configuración de seguridad almacenada en un archivo inf que se haya exportado anteriormente desde la base de datos configurada con plantillas de seguridad. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|db|Obligatorio.</br>Especifica la ruta de acceso y el nombre de una base de datos que contiene la configuración almacenada en la que se realizará la importación.</br>Si el nombre de archivo especifica una base de datos que no se ha realizado una plantilla de seguridad (tal como está representada por el archivo de configuración) asociada con él, el `/cfg \<configuration file name>` también debe especificarse la opción de línea de comandos.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad en el parámetro /cfg debe sobrescribir cualquier otra plantilla o plantilla compuesta que se almacena en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos sólo es válida cuando el `/cfg \<configuration file name>` también se usa el parámetro. Si no se especifica, la plantilla en el parámetro /cfg se anexa a la plantilla almacenada.|
|cfg|Obligatorio.</br>Especifica la ruta de acceso y el nombre de la plantilla de seguridad que se importarán en la base de datos para el análisis.</br>Esta opción /cfg solo es válida cuando se usa con el `/db \<database file name>` parámetro. Si no se especifica, el análisis se realiza contra cualquier configuración que ya está almacenada en la base de datos.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad en el parámetro /cfg debe sobrescribir cualquier otra plantilla o plantilla compuesta que se almacena en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos sólo es válida cuando el `/cfg \<configuration file name>` también se usa el parámetro. Si no se especifica, la plantilla en el parámetro /cfg se anexa a la plantilla almacenada.|
|Áreas|Opcional.</br>Especifica las áreas de seguridad que se aplican al sistema. Si no se especifica este parámetro, se aplican todas las configuraciones de seguridad definidas en la base de datos en el sistema. Para configurar varias áreas, separe cada área con un espacio. Se admiten las siguientes áreas de seguridad:</br>-   SecurityPolicy</br>    Directiva local y directiva de dominio para el sistema, incluidas las directivas de cuenta, auditan de directivas, las opciones de seguridad y así sucesivamente.</br>-   Group_Mgmt</br>    Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</br>-   User_Rights</br>    Derechos de inicio de sesión de usuario y concesión de privilegios.</br>: Claves</br>    Seguridad de las claves del registro local.</br>-   FileStore</br>    Seguridad en almacenamiento de archivos local.</br>-Servicios</br>    Seguridad para todos los servicios definidos.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|No interactivo|Opcional.</br>Suprime la salida de pantalla y de registro. Aún puede ver los resultados de análisis mediante el uso de la configuración de seguridad y análisis de complemento Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Antes de importar un archivo .inf en otro equipo, ejecute el /generaterollback secedit de comando en la base de datos en que se realizará la importación y secedit / validar en el archivo de importación para comprobar su integridad.

Si la ruta de acceso del archivo de registro no se proporciona, el archivo de registro predeterminado (*systemroot*\Documents and Settings\*UserAccount*\My Documents\Security\Logs\*DatabaseName*. se usa el registro).

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información sobre cómo actualizar la configuración de seguridad, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Ejemplos

Exportar la base de datos de seguridad y las directivas de seguridad de dominio a un archivo inf y, a continuación, importar ese archivo a otra base de datos con el fin de replicar la configuración de directiva de seguridad en otro equipo.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importar sólo la parte de las directivas de seguridad del archivo de base de datos en otro equipo.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)