---
title: secedit
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8aaa40f21c6f14dc7d686261e9980594c14a8032
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818466"
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

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Permite analizar la configuración actual de los sistemas frente a configuración de línea de base que se almacena en una base de datos.  Los resultados del análisis se almacenan en un área distinta de la base de datos y pueden verse en la configuración de seguridad y análisis.|
|[Secedit:configure](secedit-configure.md)|Le permite configurar un sistema con la configuración de seguridad almacenada en una base de datos.|
|[Secedit:export](secedit-export.md)|Permite exportar la configuración de seguridad almacenada en una base de datos.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Le permite generar una plantilla de reversión con respecto a una plantilla de configuración.|
|[Secedit:import](secedit-import.md)|Permite importar una plantilla de seguridad en una base de datos para que la configuración especificada en la plantilla se puede aplicar a un sistema o analizada en un sistema.|
|[Secedit:validate](secedit-validate.md)|Permite validar la sintaxis de una plantilla de seguridad.|

## <a name="remarks"></a>Comentarios

Para todos los nombres de archivo, se usa el directorio actual si no se especifica ninguna ruta de acceso.

Cuando se crea una plantilla de seguridad mediante el complemento de plantilla de seguridad y la configuración de seguridad y se ejecuta en el complemento de análisis, se crean los siguientes archivos:

|Archivo|Descripción|
|----|-----------|
|Scesrv.log|**Ubicación**: %windir%\security\logs</br>**Creado por**: sistema operativo</br>**Tipo de archivo**: texto</br>**Frecuencia de actualización**: Sobrescribe cuando secedit / analizar, / configurar/exportar o /import se ejecutan.</br>**Contenido**: Contiene los resultados del análisis agrupadas por tipo de directiva.|
|*Nombre de usuario seleccionado*.sdb|**Ubicación**: % windir %\*cuenta de usuario * \Documents\Security\Database</br>**Creado por**: ejecutando el complemento Configuración y análisis</br>**Tipo de archivo**: propietario</br>**Frecuencia de actualización**: Actualiza siempre que se crea una nueva plantilla de seguridad.</br>**Contenido**: Directivas de seguridad local y las plantillas de seguridad creadas por el usuario.|
|*Nombre de usuario seleccionado*.log|**Ubicación**: Definido por el usuario, pero el valor predeterminado es % windir %\*cuenta de usuario * \Documents\Security\Logs</br>**Creado por**: Ejecuta el / analyze y / configurar subcomandos (o mediante el complemento Configuración y análisis)</br>**Tipo de archivo**: texto</br>**Frecuencia de actualización**: Ejecuta el / analyze y / configurar subcomandos (o mediante el complemento Configuración y análisis); sobrescribir.</br>**Contenido**:</br>1.  Nombre de archivo de registro</br>2.  Fecha y hora</br>3.  Resultados de análisis o la investigación.|
|*Nombre de usuario seleccionado*.inf|**Ubicación**: % windir %\*cuenta de usuario * \Documents\Security\Templates</br>**Creado por**: ejecutando el complemento de plantilla de seguridad</br>**Tipo de archivo**: texto</br>**Frecuencia de actualización**: cada vez que se actualiza la plantilla de seguridad</br>**Contenido**: Contiene el conjunto de información de la plantilla para cada directiva seleccionada mediante el complemento.|

> [!NOTE]
> Microsoft Management Console (MMC) y la configuración de seguridad y complemento de análisis no están disponibles en Server Core.

#### <a name="additional-references"></a>Referencias adicionales

Para obtener ejemplos de cómo se puede usar este comando, vea la sección de ejemplos en cualquiera de los archivos subcomandos.
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)