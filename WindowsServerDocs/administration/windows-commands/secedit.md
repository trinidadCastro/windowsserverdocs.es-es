---
title: secedit
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56c9cdecfe2a5553230f1c8120c827e7f05d0ba9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821103"
---
# <a name="secedit"></a>secedit



Configura y analiza la seguridad del sistema al comparar la configuración actual con las plantillas de seguridad especificadas.

## <a name="syntax"></a>Sintaxis

```
secedit
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Permite analizar la configuración actual de los sistemas con respecto a la configuración de línea de base almacenada en una base de datos.  Los resultados del análisis se almacenan en un área independiente de la base de datos y se pueden ver en el complemento configuración y análisis de seguridad.|
|[Secedit:configure](secedit-configure.md)|Permite configurar un sistema con una configuración de seguridad almacenada en una base de datos.|
|[Secedit:export](secedit-export.md)|Permite exportar la configuración de seguridad almacenada en una base de datos.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Permite generar una plantilla de reversión con respecto a una plantilla de configuración.|
|[Secedit:import](secedit-import.md)|Permite importar una plantilla de seguridad en una base de datos de forma que la configuración especificada en la plantilla pueda aplicarse a un sistema o analizarse en un sistema.|
|[Secedit:validate](secedit-validate.md)|Permite validar la sintaxis de una plantilla de seguridad.|

## <a name="remarks"></a>Observaciones

En todos los nombres de archivo, se usa el directorio actual si no se especifica ninguna ruta de acceso.

Cuando se crea una plantilla de seguridad con el complemento plantilla de seguridad y se ejecuta el complemento configuración y análisis de seguridad, se crean los siguientes archivos:


|           Archivo           |                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv. log        |                                                                                                                             **Ubicación**:%WINDIR%\security\logs</br>**Creado por**: sistema operativo</br>**Tipo de archivo**: texto</br>**Frecuencia de actualización**: se sobrescribe cuando se ejecutan secedit/Analyze,/configure,/Export o/Import.</br>**Contenido**: contiene los resultados del análisis agrupados por tipo de directiva.                                                                                                                             |
| *Nombre seleccionado por el usuario*. sdb |                                                                                    **Ubicación**:% WINDIR% \* cuenta de usuario <em> \Documents\Security\Database</br></em>*Creado por* <em> : ejecutar el complemento configuración y análisis de seguridad</br></em>*Tipo de archivo* <em> : propietario</br></em>*Frecuencia de actualización* <em> : se actualiza cada vez que se crea una nueva plantilla de seguridad.</br></em>*Contenido* \* : directivas de seguridad local y plantillas de seguridad creadas por el usuario.                                                                                    |
| *Nombre seleccionado por el usuario*. log | **Ubicación**: definido por el usuario, pero el valor predeterminado es% WINDIR% \* cuenta de usuario <em> \Documents\Security\Logs</br></em>*Creado por* <em> : ejecutar los subcomandos/Analyze y/configure (o mediante el complemento configuración y análisis de seguridad)</br></em>*Tipo de archivo* <em> : texto</br></em>*Frecuencia de actualización* <em> : ejecutar los subcomandos/Analyze y/configure (o usar el complemento configuración y análisis de seguridad); se sobrescriben.</br></em>*Contenido* \* :</br>1. nombre de archivo de registro</br>2. fecha y hora</br>3. resultados del análisis o de la investigación. |
| *Nombre seleccionado por el usuario*. inf |                                                                                     **Ubicación**:% WINDIR% \* cuenta de usuario <em> \Documents\Security\Templates</br></em>*Creado por* <em> : ejecutar el complemento de plantilla de seguridad</br></em>*Tipo de archivo* <em> : texto</br></em>*Frecuencia de actualización* <em> : cada vez que se actualiza la plantilla de seguridad</br></em>*Contenido* \* : contiene la información de configuración de la plantilla para cada directiva seleccionada mediante el complemento.                                                                                     |

> [!NOTE]
> Microsoft Management Console (MMC) y el complemento configuración y análisis de seguridad no están disponibles en Server Core.

## <a name="additional-references"></a>Referencias adicionales

Para obtener ejemplos de cómo se puede usar este comando, consulte la sección ejemplos en cualquiera de los archivos de subcomando.
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)