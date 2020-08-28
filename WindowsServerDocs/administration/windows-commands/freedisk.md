---
title: freedisk
description: Artículo de referencia para el comando FREEDISK, que comprueba si la cantidad especificada de espacio en disco está disponible antes de continuar con un proceso de instalación.
ms.topic: reference
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a3f8543e6fd2cff9a4e086068155d84526d9377
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027613"
---
# <a name="freedisk"></a>freedisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba si la cantidad especificada de espacio en disco está disponible antes de continuar con un proceso de instalación.

## <a name="syntax"></a>Sintaxis

```
freedisk [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] [/d <drive>] [<value>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificados en el comando. |
| /u `[<domain>\]<user>` | Ejecuta el script con los permisos de la cuenta de usuario especificada. El valor predeterminado es los permisos del sistema. |
| /p [ <password> ] | Especifica la contraseña de la cuenta de usuario especificada en **/u**. |
| /d. `<drive>` | Especifica la unidad para la que desea averiguar la disponibilidad de espacio disponible. Debe especificar `<drive>` para un equipo remoto. |
| `<value>` | Comprueba una cantidad específica de espacio libre en disco. Puede especificar `<value>` en bytes, KB, MB, GB, TB, PB, EB, ZB o YB. |

#### <a name="remarks"></a>Observaciones

- El uso de las opciones de línea de comandos **/s**, **/u**y **/p** solo está disponible cuando se utiliza **/s**. Debe usar **/p** con **/u**para proporcionar la contraseña del usuario.

- En el caso de las instalaciones desatendidas, puede usar **FREEDISK** en archivos por lotes de instalación para comprobar el espacio disponible en la cantidad de requisitos previos antes de continuar con la instalación.

- Cuando se usa **FREEDISK** en un archivo por lotes, devuelve un **0** si hay espacio suficiente y un **1** si no hay espacio suficiente.

### <a name="examples"></a>Ejemplos

Para determinar si hay al menos 50 MB de espacio libre disponible en la unidad C:, escriba:

```
freedisk 50mb
```

En la pantalla aparece un resultado similar al siguiente:

```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
