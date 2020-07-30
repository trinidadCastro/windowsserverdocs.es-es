---
title: ReFSUtil
description: Artículo de referencia de la herramienta ReFSUtil, que intenta diagnosticar grandes volúmenes de ReFS dañados, identificar archivos restantes y copiar esos archivos en otro volumen.
author: laknight5
ms.author: laknight
ms.date: 6/29/2020
ms.prod: windows-server
ms.technology: windows-commands
ms.topic: article
ms.openlocfilehash: 3afc96970bb0350a3c1168c520cc20ad4f2254af
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409726"
---
# <a name="refsutil"></a>ReFSUtil

> Se aplica a: Windows Server 2019, Windows 10

ReFSUtil es una herramienta incluida en Windows y Windows Server que intenta diagnosticar grandes volúmenes de ReFS dañados, identificar archivos restantes y copiar esos archivos en otro volumen. Esto viene en Windows 10 en la carpeta `%SystemRoot%\Windows\System32` o en Windows Server en la `%SystemRoot%\System32` carpeta.

ReFS salvage es la función principal de ReFSUtil y es útil para recuperar datos de volúmenes que se muestran como sin procesar en administración de discos. ReFS salvage tiene dos fases: fase de examen y una fase de copia. En el modo automático, la fase de exploración y la fase de copia se ejecutarán secuencialmente. En el modo manual, cada fase se puede ejecutar por separado. El progreso y los registros se guardan en un directorio de trabajo para permitir que las fases se ejecuten por separado, así como la fase de análisis que se va a pausar y reanudar. No es necesario usar la herramienta ReFSutil a menos que el volumen sea RAW. Si es de solo lectura, los datos siguen siendo accesibles.

## <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<source volume>` | Especifica el volumen de ReFS que se va a procesar. La letra de la unidad debe tener el formato "L:", o debe proporcionar una ruta de acceso al punto de montaje del volumen. |
| `<working directory>` | Especifica la ubicación donde se almacenará la información temporal y los registros. **No** debe estar ubicado en `<source volume>` . |
| `<target directory>` | Especifica la ubicación en la que se copian los archivos identificados. **No** debe estar ubicado en `<source volume>` . |
| \- m | Recupera todos los archivos posibles, incluidos los eliminados.<p>**ADVERTENCIA:** Este parámetro no solo hace que el proceso tarde más en ejecutarse, pero también puede provocar resultados inesperados. |
| \-v | Especifica que se debe usar el modo detallado. |
| \-x | Fuerza el desmontaje del volumen en primer lugar, si es necesario. Todos los identificadores abiertos del volumen no son válidos. Por ejemplo, `refsutil salvage -QA R: N:\WORKING N:\DATA -x`. |

## <a name="usage-and-available-options"></a>Opciones de uso y disponibles

### <a name="quick-automatic-mode-command-line-usage"></a>Uso rápido de la línea de comandos de modo automático

Realiza una fase de examen rápido seguida de una fase de copia. Este modo se ejecuta más rápido, ya que supone que algunas estructuras críticas del volumen no están dañadas y, por lo tanto, no es necesario examinar todo el volumen para localizarlas. Esto también reduce la recuperación de archivos, directorios o volúmenes obsoletos.

```
refsutil salvage -QA <source volume> <working directory> <target directory> <options>
```

### <a name="full-automatic-mode-command-line-usage"></a>Uso completo de la línea de comandos de modo automático

Realiza una fase de examen completo seguida de una fase de copia. Este modo puede tardar mucho tiempo, ya que examinará todo el volumen para detectar archivos, directorios o volúmenes recuperables.

```
refsutil salvage -FA <source volume> <working directory> <target directory> <options>
```

### <a name="diagnose-phase-command-line-usage-manual-mode"></a>Uso de la línea de comandos de fase de diagnóstico (modo manual)

En primer lugar, intente determinar si `<source volume>` es un volumen ReFS y determine si el volumen se va a montar. Si un volumen no se puede montar, se proporcionarán los motivos. Se trata de una fase independiente.

```
refsutil salvage -D <source volume> <working directory> <options>
```

### <a name="quick-scan-phase-command-line-usage"></a>Uso de la línea de comandos de la fase de examen rápido

Realiza un examen rápido del `<source volume>` para cualquier archivo recuperable. Este modo se ejecuta más rápido, ya que supone que algunas estructuras críticas del volumen no están dañadas y, por lo tanto, no es necesario examinar todo el volumen para localizarlas. Esto también reduce la recuperación de archivos, directorios o volúmenes obsoletos. Los archivos detectados se registran en el `foundfiles.<volume signature>.txt` archivo, que se encuentra en el `<working directory>` . Si la fase de exploración se detuvo previamente, al ejecutar con la marca **-q** vuelve a reanudar el análisis desde donde se quedó.

```
refsutil salvage -QS <source volume> <working directory> <options>
```

### <a name="full-scan-phase-command-line-usage"></a>Uso de la línea de comandos de la fase de examen completo

Examina toda la totalidad `<source volume>` de los archivos recuperables. Este modo puede tardar mucho tiempo, ya que examinará todo el volumen en busca de archivos recuperables. Los archivos detectados se registrarán en el `foundfiles.<volume signature>.txt` archivo, que se encuentra en el `<working directory>` . Si la fase de exploración se detuvo previamente, al ejecutar con la marca **-FS** se reanuda el análisis desde donde se quedó.

```
refsutil salvage -FS <source volume> <working directory> <options>
```

### <a name="copy-phase-command-line-usage"></a>Uso de la línea de comandos de la fase de copia

Copia todos los archivos descritos en el archivo en el `foundfiles.<volume signature>.txt` `<target directory>` . Si detiene la fase de análisis demasiado pronto, es posible que el `foundfiles.<volume signature>.txt` archivo no exista todavía, por lo que no se copia ningún archivo en `<target directory>` .

```
refsutil salvage -C <source volume> <working directory> <target directory> <options>
```

### <a name="copy-phase-with-list-command-line-usage"></a>Copiar fase con el uso de la línea de comandos de lista

Copia todos los archivos de `<file list>` de la `<source volume>` a su `<target directory>` . Los archivos de `<file list>` deben haber sido identificados primero por la fase de análisis, aunque no es necesario que el examen se haya ejecutado hasta su finalización. `<file list>`Se puede generar copiando `foundfiles.<volume signature>.txt` en un archivo nuevo, quitando las líneas que hacen referencia a los archivos que no deben restaurarse y conservando los archivos que deben restaurarse. El cmdlet **Select-String** de PowerShell puede ser útil para filtrar `foundfiles.<volume signature>.txt` solo para incluir rutas de acceso, extensiones o nombres de archivo deseados.

```
refsutil salvage -SL <source volume> <working directory> <target directory> <file list> <options>
```

### <a name="copy-phase-with-interactive-console"></a>Copia de la fase con la consola interactiva

Los usuarios avanzados pueden rescatar los archivos mediante una consola interactiva. Este modo también requiere archivos generados en cualquiera de las fases de examen.

```
refsutil salvage -IC <source volume> <working directory> <options>
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
