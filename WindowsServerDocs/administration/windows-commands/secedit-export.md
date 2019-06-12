---
title: secedit:export
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398d2fa47f2418aec910569c2eb85aec408ad482
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441593"
---
# <a name="seceditexport"></a>secedit:export



Exporta la configuración de seguridad almacenada en una base de datos configurada con plantillas de seguridad. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|db|Obligatorio.</br>Especifica la ruta de acceso y el nombre de una base de datos que contiene la configuración almacenada en la que se realizará el análisis.</br>Si el nombre de archivo especifica una base de datos que no se ha realizado una plantilla de seguridad (tal como está representada por el archivo de configuración) asociada con él, el `/cfg \<configuration file name>` también debe especificarse la opción de línea de comandos.|
|mergedpolicy|Opcional.</br>Combina y exporta el dominio y la configuración de seguridad de la directiva local.|
|cfg|Obligatorio.</br>Especifica la ruta de acceso y el nombre de la plantilla de seguridad que se importarán en la base de datos para el análisis.</br>Esta opción /cfg solo es válida cuando se usa con el `/db \<database file name>` parámetro. Si no se especifica, el análisis se realiza contra cualquier configuración que ya está almacenada en la base de datos.|
|Áreas|Opcional.</br>Especifica las áreas de seguridad que se aplican al sistema. Si no se especifica este parámetro, se aplican todas las configuraciones de seguridad definidas en la base de datos en el sistema. Para configurar varias áreas, separe cada área con un espacio. Se admiten las siguientes áreas de seguridad:</br>-   SecurityPolicy</br>    Directiva local y directiva de dominio para el sistema, incluidas las directivas de cuenta, auditan de directivas, las opciones de seguridad y así sucesivamente.</br>-   Group_Mgmt</br>    Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</br>-   User_Rights</br>    Derechos de inicio de sesión de usuario y concesión de privilegios.</br>: Claves</br>    Seguridad de las claves del registro local.</br>-   FileStore</br>    Seguridad en almacenamiento de archivos local.</br>-Servicios</br>    Seguridad para todos los servicios definidos.|
|log|Opcional.</br>Especifica la ruta de acceso y el nombre del archivo de registro para el proceso.|
|No interactivo|Opcional.</br>Suprime la salida de pantalla y de registro. Aún puede ver los resultados de análisis mediante el uso de la configuración de seguridad y análisis de complemento Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentarios

Puede usar este comando copia de seguridad de las directivas de seguridad en un equipo local además de importar la configuración a otro equipo.

Si la ruta de acceso del archivo de registro no se proporciona, el archivo de registro predeterminado (*systemroot*\Documents and Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>. se usa el registro).

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información sobre cómo actualizar la configuración de seguridad, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Ejemplos

Exportar la base de datos de seguridad y las directivas de seguridad de dominio a un archivo inf y, a continuación, importar ese archivo a otra base de datos con el fin de replicar la configuración de directiva de seguridad en otro equipo.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importar ese archivo de base de datos en otro equipo.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)