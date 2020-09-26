---
title: secedit configure
description: Artículo de referencia del comando secedit configure, que le permite configurar las opciones actuales del sistema mediante la configuración de seguridad almacenada en una base de datos.
ms.topic: reference
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 39f69651a69a748acf65727ecec0bcbe4b9c911a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388228"
---
# <a name="secedit-configure"></a>secedit/configure

Permite configurar las opciones actuales del sistema mediante la configuración de seguridad almacenada en una base de datos.

## <a name="syntax"></a>Sintaxis

```
secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /dB | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la base de datos que contiene la configuración almacenada. Si el nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) asociada, `/cfg <configuration file name>` también se debe especificar la opción. |
| /cfg | Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis. Esta opción solo es válida cuando se usa con el `/db <database file name>` parámetro. Si este parámetro no se especifica también, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos. |
| /overwrite | Especifica si la plantilla de seguridad del parámetro **/cfg** debe sobrescribir cualquier plantilla o plantilla compuesta almacenada en la base de datos, en lugar de anexar los resultados a la plantilla almacenada. Esta opción solo es válida cuando `/cfg <configuration file name>` se usa también el parámetro. Si este parámetro no se especifica también, la plantilla del parámetro **/cfg** se anexa a la plantilla almacenada. |
| /Areas | Especifica las áreas de seguridad que se van a aplicar al sistema. Si no se especifica este parámetro, se aplicará al sistema toda la configuración de seguridad definida en la base de datos. Para configurar varias áreas, separe cada área por un espacio. Se admiten las siguientes áreas de seguridad:<ul><li>**SecurityPolicy:** Directiva local y Directiva de dominio para el sistema, incluidas las directivas de cuenta, las directivas de auditoría, las opciones de seguridad, etc.</li><li>  **group_mgmt:** Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</li><li>**USER_RIGHTS:** Derechos de inicio de sesión de usuario y concesión de privilegios.</li><li>**REGKEYS:** Seguridad de las claves del registro local.</li><li>**almacén de almacén:** Seguridad en el almacenamiento de archivos local.</li><li>**servicios:** Seguridad para todos los servicios definidos.</li></ul> |
| /log | Especifica la ruta de acceso y el nombre del archivo de registro que se va a utilizar en el proceso. Si no especifica una ubicación de archivo, se utiliza el archivo de registro predeterminado `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` . |
| /quiet | Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC). |

## <a name="examples"></a>Ejemplos

Para realizar el análisis de los parámetros de seguridad en la base de datos de seguridad, *SecDbContoso. sdb*y dirigir la salida al archivo *SecAnalysisContosoFY11*, incluidos los mensajes para comprobar que el comando se ejecutó correctamente, escriba:

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

Para incorporar los cambios requeridos por el proceso de análisis en el archivo *SecContoso. inf* y, a continuación, dirigir la salida al archivo existente, *SecAnalysisContosoFY11*, sin preguntar, escriba:

```
secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [secedit/ANALYZE](secedit-analyze.md)

- [secedit/Export](secedit-export.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)

- [secedit/Validate](secedit-validate.md)