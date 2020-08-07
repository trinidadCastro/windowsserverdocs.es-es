---
title: regini
description: Artículo de referencia para el comando Regini, que modifica el registro desde la línea de comandos o un script, y aplica los cambios predefinidos en uno o varios archivos de texto.
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d0653ca97ca4401cb24250bbce1c627b6c683982
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883968"
---
# <a name="regini"></a>regini

Modifica el registro desde la línea de comandos o un script, y aplica los cambios predefinidos en uno o varios archivos de texto. Puede crear, modificar o eliminar claves del registro, además de modificar los permisos en las claves del registro.

Para obtener información detallada sobre el formato y el contenido del archivo de script de texto que regini.exe utiliza para realizar cambios en el registro, vea [Cómo cambiar los valores o permisos del registro desde una línea de comandos o un script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Sintaxis

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputwidth][-b] textfiles...
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -m `<\\computername>` | Especifica el nombre del equipo remoto con un registro que se va a modificar. Use el formato ** \\ ComputerName**. |
| -h`<hivefile hiveroot>` | Especifica el subárbol del registro local que se va a modificar. Debe especificar el nombre del archivo de Hive y la raíz del subárbol en el formato **hivefile hiveroot**. |
| -i`<n>` | Especifica el nivel de sangría que se va a utilizar para indicar la estructura de árbol de las claves del registro en el resultado del comando. La herramienta **regdmp.exe** (que obtiene los permisos actuales de una clave del registro en formato binario) utiliza la sangría en múltiplos de cuatro, por lo que el valor predeterminado es **4**. |
| -o`<outputwidth>` | Especifica el ancho del resultado del comando, en caracteres. Si el resultado aparecerá en la ventana de comandos, el valor predeterminado es el ancho de la ventana. Si el resultado se dirige a un archivo, el valor predeterminado es **240** caracteres. |
| -b | Especifica que **regini.exe** resultado es compatible con versiones anteriores de **regini.exe**. |
| textfiles | Especifica el nombre de uno o más archivos de texto que contienen datos del registro. Se puede enumerar cualquier número de archivos de texto ANSI o Unicode. |

#### <a name="remarks"></a>Observaciones

Las siguientes directrices se aplican principalmente al contenido de los archivos de texto que contienen los datos del registro que se aplican mediante **regini.exe**.

- Use el punto y coma como un carácter de comentario de final de línea. Debe ser el primer carácter que no esté en blanco en una línea.

- Use la barra diagonal inversa para indicar la continuación de una línea. El comando omitirá todos los caracteres de la barra diagonal inversa hasta el primer carácter que no esté en blanco de la línea siguiente, pero no lo incluya. Si incluye más de un espacio antes de la barra diagonal inversa, se reemplaza por un solo espacio.

- Use caracteres de tabulación para controlar la sangría. Esta sangría indica la estructura de árbol de las claves del registro; sin embargo, estos caracteres se convierten en un solo espacio, independientemente de su posición.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
