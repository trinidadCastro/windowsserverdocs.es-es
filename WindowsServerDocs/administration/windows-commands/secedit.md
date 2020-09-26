---
title: comandos Secedit
description: Artículo de referencia para los comandos de secedit, que comparan las configuraciones de seguridad actuales con las plantillas de seguridad especificadas.
ms.topic: reference
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ceb1a28376c17ab9d08689c7b0367dd90fdecc4f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388292"
---
# <a name="secedit-commands"></a>comandos Secedit

Configura y analiza la seguridad del sistema comparando la configuración de seguridad actual con las plantillas de seguridad especificadas.

> [!NOTE]
> Microsoft Management Console (MMC) y el complemento configuración y análisis de seguridad no están disponibles en Server Core.

## <a name="syntax"></a>Sintaxis

```
secedit /analyze
secedit /configure
secedit /export
secedit /generaterollback
secedit /import
secedit /validate
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [secedit/ANALYZE](secedit-analyze.md) | Permite analizar la configuración actual de los sistemas con respecto a la configuración de línea de base almacenada en una base de datos.  Los resultados del análisis se almacenan en un área independiente de la base de datos y se pueden ver en el complemento configuración y análisis de seguridad. |
| [secedit/configure](secedit-configure.md) | Permite configurar un sistema con una configuración de seguridad almacenada en una base de datos. |
| [secedit/Export](secedit-export.md) | Permite exportar la configuración de seguridad almacenada en una base de datos. |
| [secedit/generaterollback](secedit-generaterollback.md) | Permite generar una plantilla de reversión con respecto a una plantilla de configuración. |
| [secedit/Import](secedit-import.md) | Permite importar una plantilla de seguridad en una base de datos de forma que la configuración especificada en la plantilla pueda aplicarse a un sistema o analizarse en un sistema. |
| [secedit/Validate](secedit-validate.md) | Permite validar la sintaxis de una plantilla de seguridad. |

#### <a name="remarks"></a>Observaciones

- Si no se especifica FilePath, todos los nombres de archivo tendrán como valor predeterminado el directorio actual.

- Los resultados del análisis se almacenan en un área independiente de la base de datos y se pueden ver en el complemento configuración y análisis de seguridad de MMC.

- Si las plantillas de seguridad se crean mediante el complemento plantilla de seguridad y, si ejecuta el complemento configuración y análisis de seguridad en esas plantillas, se crean los siguientes archivos:

    | Archivo | Descripción |
    |--|--|
    | scesrv. log | <ul><li>**Ubicación:**`%windir%\security\logs`</li><li>**Creado por:** Sistema operativo</li><li>**Tipo de archivo:** Negrita</li><li>**Frecuencia de actualización:** Se sobrescribe cuando `secedit analyze` `secedit configure` `secedit export` `secedit import` se ejecuta, o.</li><li>**Contenido:** Contiene los resultados del análisis agrupados por tipo de directiva.</li></ul> |
    | *nombre seleccionado por el usuario*. sdb | <ul><li>**Ubicación:**`%windir%\<user account>\Documents\Security\Database`</li><li>**Creado por:** Ejecutar el complemento configuración y análisis de seguridad</li><li>**Tipo de archivo:** Complementaria</li><li>**Frecuencia de actualización:** Se actualiza cada vez que se crea una nueva plantilla de seguridad.</li><li>**Contenido:** Directivas de seguridad local y plantillas de seguridad creadas por el usuario.</li></ul> |
    | *nombre seleccionado por el usuario*. log | <ul><li>**Ubicación:** Definido por el usuario, pero tiene como valor predeterminado `%windir%\<user account>\Documents\Security\Logs`</li><li>**Creado por:** Ejecutar los `secedit analyze` `secedit configure` comandos o, o mediante el complemento configuración y análisis de seguridad.</li><li>**Tipo de archivo:** Negrita</li><li>**Frecuencia de actualización:** Se sobrescribe cuando `secedit analyze` `secedit configure` se ejecuta o, o mediante el complemento configuración y análisis de seguridad.</li><li>**Contenido:** Nombre del archivo de registro, fecha y hora, y los resultados del análisis o la investigación.</li></ul> |
    | *nombre seleccionado por el usuario*. inf | <ul><li>**Ubicación:**`%windir%\*<user account>\Documents\Security\Templates`</li><li>**Creado por:** Ejecutar el complemento de plantilla de seguridad.</li><li>**Tipo de archivo:** Negrita</li><li>**Frecuencia de actualización:** Se sobrescribe cada vez que se actualiza la plantilla de seguridad.</li><li>**Contenido:** Contiene la información de configuración de la plantilla para cada directiva seleccionada mediante el complemento.</li></ul> |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
