---
title: secedit export
description: Artículo de referencia para la exportación de secedit, que exporta la configuración de seguridad almacenada en una base de datos configurada con plantillas de seguridad.
ms.topic: reference
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dfd766674a4e4ac2e9552d36b4cb706117c173bd
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388248"
---
# <a name="secedit-export"></a>secedit/Export

Exporta la configuración de seguridad almacenada en una base de datos configurada con plantillas de seguridad. Puede usar este comando para hacer una copia de seguridad de las directivas de seguridad en un equipo local, además de importar la configuración en otro equipo.

## <a name="syntax"></a>Sintaxis

```
secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /dB | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la base de datos que contiene la configuración almacenada en la que se realiza la exportación. Si el nombre de archivo especifica una base de datos que no tiene una plantilla de seguridad (tal como la representa el archivo de configuración) asociada, `/cfg <configuration file name>` también se debe especificar la opción. |
| /mergedpolicy | Combina y exporta la configuración de seguridad de la Directiva de dominio y local. |
| /cfg | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se importará en la base de datos para su análisis. Esta opción solo es válida cuando se usa con el `/db <database file name>` parámetro. Si este parámetro no se especifica también, el análisis se realiza en cualquier configuración que ya esté almacenada en la base de datos. |
| /Areas | Especifica las áreas de seguridad que se van a aplicar al sistema. Si no se especifica este parámetro, se aplicará al sistema toda la configuración de seguridad definida en la base de datos. Para configurar varias áreas, separe cada área por un espacio. Se admiten las siguientes áreas de seguridad:<ul><li>**SecurityPolicy:** Directiva local y Directiva de dominio para el sistema, incluidas las directivas de cuenta, las directivas de auditoría, las opciones de seguridad, etc.</li><li>  **group_mgmt:** Configuración de grupo restringido para los grupos especificados en la plantilla de seguridad.</li><li>**USER_RIGHTS:** Derechos de inicio de sesión de usuario y concesión de privilegios.</li><li>**REGKEYS:** Seguridad de las claves del registro local.</li><li>**almacén de almacén:** Seguridad en el almacenamiento de archivos local.</li><li>**servicios:** Seguridad para todos los servicios definidos.</li></ul> |
| /log | Especifica la ruta de acceso y el nombre del archivo de registro que se va a utilizar en el proceso. Si no especifica una ubicación de archivo, se utiliza el archivo de registro predeterminado `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` . |
| /quiet | Suprime la salida de la pantalla y del registro. Todavía puede ver los resultados del análisis mediante el complemento configuración y análisis de seguridad de Microsoft Management Console (MMC). |

## <a name="examples"></a>Ejemplos

Para exportar la base de datos de seguridad y las directivas de seguridad de dominio a un archivo INF y, a continuación, importar ese archivo a una base de datos diferente para replicar la configuración de la Directiva de seguridad en otro equipo, escriba:

```
secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```

Para importar el archivo de ejemplo en una base de datos diferente en otro equipo, escriba:

```
secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [secedit/ANALYZE](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)

- [secedit/Validate](secedit-validate.md)