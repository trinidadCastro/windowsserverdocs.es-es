---
title: secedit:configure
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9420945dca9b72de1937258201e7072d2bb115b2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441525"
---
# <a name="seceditconfigure"></a>secedit:configure



Le permite configurar la configuración actual del sistema con la configuración de seguridad almacenada en una base de datos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|db|Obligatorio.</br>Especifica la ruta de acceso y el nombre de una base de datos que contiene la configuración almacenada.</br>Si el nombre de archivo especifica una base de datos que no se ha realizado una plantilla de seguridad (tal como está representada por el archivo de configuración) asociada con él, el `/cfg \<configuration file name>` también debe especificarse la opción de línea de comandos.|
|cfg|Opcional.</br>Especifica la ruta de acceso y el nombre de la plantilla de seguridad que se importarán en la base de datos para el análisis.</br>Esta opción /cfg solo es válida cuando se usa con el `/db \<database file name>` parámetro. Si no se especifica, el análisis se realiza contra cualquier configuración que ya está almacenada en la base de datos.|
|sobrescribir|Opcional.</br>Especifica si la plantilla de seguridad en el parámetro /cfg debe sobrescribir cualquier otra plantilla o plantilla compuesta que se almacena en la base de datos en lugar de anexar los resultados a la plantilla almacenada.</br>Esta opción de línea de comandos sólo es válida cuando el `/cfg \<configuration file name>` también se usa el parámetro. Si no se especifica, la plantilla en el parámetro /cfg se anexa a la plantilla almacenada.|
|Áreas|Opcional.</br>Especifica las áreas de seguridad que se aplican al sistema. Si no se especifica este parámetro, se aplican todas las configuraciones de seguridad definidas en la base de datos en el sistema. Para configurar varias áreas, separe cada área con un espacio. Se admiten las siguientes áreas de seguridad:</br>-   SecurityPolicy</br>    Directiva local y directiva de dominio para el sistema, incluidas las directivas de cuenta, auditan de directivas, las opciones de seguridad y así sucesivamente.</br>-   Group_Mgmt</br>    Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</br>-   User_Rights</br>    Derechos de inicio de sesión de usuario y concesión de privilegios.</br>: Claves</br>    Seguridad de las claves del registro local.</br>-   FileStore</br>    Seguridad en almacenamiento de archivos local.</br>-Servicios</br>    Seguridad para todos los servicios definidos.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|No interactivo|Opcional.</br>Suprime la salida de pantalla y de registro. Aún puede ver los resultados de análisis mediante el uso de la configuración de seguridad y análisis de complemento Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Si la ruta de acceso del archivo de registro no se proporciona, el archivo de registro predeterminado (*systemroot*\Users \*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>.log) se utiliza.

Con Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información sobre cómo actualizar la configuración de seguridad, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Ejemplos

Realizar el análisis de los parámetros de seguridad en la base de datos de seguridad, SecDbContoso.sdb, que ha creado mediante la configuración de seguridad y análisis. Dirigir la salida al archivo SecAnalysisContosoFY11 pedir confirmación para que pueda comprobar que el comando se ejecutó correctamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Supongamos que el análisis reveló algunas deficiencias, por lo que se modificó la plantilla de seguridad, SecContoso.inf. Ejecute el comando nuevo para incorporar los cambios, dirigir la salida al archivo SecAnalysisContosoFY11 existente sin pedir confirmación.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)