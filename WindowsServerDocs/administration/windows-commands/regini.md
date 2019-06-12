---
title: regini
description: Obtenga información sobre cómo modificar el registro desde el símbolo del sistema o mediante un script.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: e3f8454572b662c9327aeb4783c5e9651ad2022b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441888"
---
# <a name="regini"></a>regini

Modifica el registro de la línea de comandos o una secuencia de comandos y aplica los cambios que se preestablece en uno o varios archivos de texto. Puede crear, modificar o eliminar las claves del registro, además de modificar los permisos en las claves del registro.

Para obtener más información sobre el formato y el contenido del archivo de script de texto que usa Regini.exe para realizar cambios en el registro, consulte [cómo cambiar los permisos o los valores del registro desde una línea de comandos o una secuencia de comandos](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Sintaxis

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |

|-m \< \\ \\nombreDeEquipo >|Especifica el nombre del equipo remoto con un registro que se va a modificar. Use el formato  **\\ \\ComputerName**.|
|---------------------|-|
|-h \<hivefile raízDeSección >|Especifica el subárbol del registro local para modificar. Debe especificar el nombre del archivo de hive y la raíz del subárbol en el formato **hivefile raízDeSección**.|
|-i \<n>|Especifica el nivel de sangría se utiliza para indicar la estructura de árbol de las claves del registro en la salida del comando. El **Regdmp.exe** herramienta (que obtiene los permisos actuales de la clave de un registro en formato binario) utiliza la sangría en múltiplos de cuatro, por lo que el valor predeterminado es **4**.|
|-o \<anchoDeSalida >|Especifica el ancho de la salida del comando, en caracteres. Si el resultado aparecerá en la ventana de comandos, el valor predeterminado es el ancho de la ventana. Si la salida se dirige a un archivo, el valor predeterminado es **240** caracteres.|
|-b|Especifica que **Regini.exe** salida es compatible con versiones anteriores de **Regini.exe**. Vea la sección Comentarios para obtener más información.|
|textfiles|Especifica el nombre de uno o varios archivos de texto que contienen datos del registro. Puede aparecer cualquier número de archivos de texto ANSI o Unicode.|

## <a name="remarks"></a>Comentarios

Las siguientes directrices se aplican principalmente para el contenido de los archivos de texto que contienen datos del registro que se aplican mediante **Regini.exe**.
-   Utilice el punto y coma como un carácter de comentario de final de línea. Debe ser el primer carácter no en blanco en una línea.
-   Use la barra diagonal inversa para indicar la continuación de una línea. El comando pasará por alto todos los caracteres de barra diagonal inversa hasta (pero no incluyendo) el primer carácter no en blanco de la línea siguiente. Si incluye más de un espacio antes de la barra diagonal inversa, se reemplaza por un solo espacio.
-   Usar disco duro tabulaciones para controlar la sangría. Esta sangría indica la estructura de árbol de las claves del registro; Sin embargo, estos caracteres se convierten en un único espacio independientemente de su posición.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)