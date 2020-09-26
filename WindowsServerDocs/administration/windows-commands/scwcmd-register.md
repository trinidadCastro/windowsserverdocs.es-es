---
title: scwcmd register
description: Artículo de referencia para el comando scwcmd Register, que extiende o Personaliza la base de datos de configuración de seguridad del Asistente para configuración de seguridad (SCW) mediante el registro de un archivo de base de datos de configuración de seguridad que contiene definiciones de roles, tareas, servicios o puertos.
ms.topic: reference
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b1e981ef2a428174406fd793d5c959de57a8067c
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388581"
---
# <a name="scwcmd-register"></a>scwcmd register

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

Extiende o Personaliza la base de datos de configuración de seguridad del Asistente para configuración de seguridad (SCW) registrando un archivo de base de datos de configuración de seguridad que contiene definiciones de roles, tareas, servicios o puertos.

## <a name="syntax"></a>Sintaxis

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /kbname:`<MyApp>` | Especifica el nombre con el que se registrará la extensión de la base de datos de configuración de seguridad. Se debe especificar este parámetro. |
| /kbfile:`<kb.xml>` | Especifica la ruta de acceso y el nombre del archivo de base de datos de configuración de seguridad que se usa para extender o personalizar la base de datos de configuración de seguridad base. Para validar que el archivo de base de datos de configuración de seguridad es compatible con el esquema de SCW, use el `%windir%\security\KBRegistrationInfo.xsd` archivo de definición de esquema. Se debe proporcionar esta opción a menos que se especifique el parámetro **/d** . |
| ó`<path>` | Especifica la ruta de acceso al directorio que contiene los archivos de base de datos de configuración de seguridad de SCW que se van a actualizar. Si no se especifica esta opción, `%windir%\security\msscw\kbs` se utiliza. |
| /d | Anula el registro de una extensión de base de datos de configuración de seguridad de la base de datos de configuración de seguridad. La extensión a la que se va a anular el registro se especifica mediante el parámetro **/kbname** . (No se debe especificar el parámetro **/KBFile** ). La base de datos de configuración de seguridad de la que se va a anular el registro de la extensión se especifica mediante el parámetro **/KB** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para registrar el archivo de base de datos de configuración de seguridad denominado *SCWKBForMyApp.xml* bajo el nombre *MyApp* en la ubicación `\\kbserver\kb` , escriba:

```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```

Para anular el registro de la base de datos de configuración de seguridad *MyApp*, ubicada en `\\kbserver\kb` , escriba:

```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando scwcmd ANALYZE](scwcmd-analyze.md)

- [comando scwcmd configure](scwcmd-configure.md)

- [comando de restauración de scwcmd](scwcmd-rollback.md)

- [scwcmd transform (comando)](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)