---
title: bootcfg raw
description: Tema de referencia para el comando bootcfg RAW, que agrega opciones de carga del sistema operativo, especificadas como una cadena, a una entrada del sistema operativo en la sección sistema operativo del archivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9127b278a41bb8f1f36f7b45c544bf09f5c4ff7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708954"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada del sistema operativo en la sección [operating systems] del archivo boot. ini. Este comando sobrescribe las opciones de entrada existentes del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>`. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `<osloadoptionsstring>` | Especifica las opciones de carga del sistema operativo que se van a agregar a la entrada de sistema operativo. Estas opciones de carga reemplazan las opciones de carga existentes asociadas a la entrada del sistema operativo. No hay ninguna validación en el `<osloadoptions>` parámetro.
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /a | Especifica qué opciones del sistema operativo se deben anexar a las opciones del sistema operativo existentes. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Este texto debe contener opciones de carga del sistema operativo válidas como **/Debug**, **/fastdetect**, **/nodebug**, **/Baudrate**, **/crashdebug**y **/SOS**.

Para agregar **/Debug/fastdetect** al final de la primera entrada de sistema operativo, reemplazando las opciones de entrada de sistema operativo anteriores:

```
bootcfg /raw /debug /fastdetect /id 1
```

Para usar el comando **bootcfg/RAW** :

```
bootcfg /raw /debug /sos /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
