---
title: ReFSUtil
description: ReFSUtil es una herramienta incluida en Windows y Windows Server que intenta diagnosticar grandes volúmenes de ReFS dañados, identificar archivos restantes y copiar esos archivos en otro volumen.
author: laknight5
ms.author: laknight
ms.date: 6/29/2020
ms.prod: windows-server
ms.technology: storage-file-systems
ms.topic: article
ms.openlocfilehash: 0d7c1bf79695c2ddd03943744ebf1df4827d365c
ms.sourcegitcommit: 457e88e5aa6be13a2bffdb8e434a8efc3698678f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549170"
---
# <a name="refsutil"></a>ReFSUtil

>Se aplica a: Windows Server 2019, Windows 10

ReFSUtil es una herramienta incluida en Windows y Windows Server que intenta diagnosticar grandes volúmenes de ReFS dañados, identificar archivos restantes y copiar esos archivos en otro volumen. Esto viene en Windows 10 en la carpeta%SystemRoot%\Windows\System32 o en Windows Server en la carpeta% SystemRoot% \\ system32.

ReFS salvage es la función principal de ReFSUtil en la actualidad y es útil para recuperar los datos de los volúmenes que se muestran como sin procesar en la administración de discos. ReFS salvage tiene dos fases: fase de examen y una fase de copia. En el modo automático, la fase de exploración y la fase de copia se ejecutarán secuencialmente. En el modo manual, cada fase se puede ejecutar por separado. El progreso y los registros se guardan en un directorio de trabajo para permitir que las fases se ejecuten por separado, así como la fase de análisis que se va a pausar y reanudar. No es necesario usar ReFSutil a menos que el volumen sea RAW. Si es de solo lectura, los datos siguen siendo accesibles.

## <a name="automatic-mode-command-line-usage"></a>Uso de la línea de comandos de modo automático

### <a name="quick-automatic-mode-command-line-usage"></a>Uso rápido de la línea de comandos de modo automático

```dos
refsutil salvage -QA <source volume> <working directory> <target directory> <options>
```
Realizará una fase de examen rápido seguida de una fase de copia. Este modo se ejecuta más rápido, ya que supone que algunas estructuras críticas del volumen no están dañadas y, por lo tanto, no es necesario examinar todo el volumen para localizarlas. Esto también reduce la recuperación de archivos, directorios o volúmenes obsoletos.

### <a name="full-automatic-mode-command-line-usage"></a>Uso completo de la línea de comandos de modo automático

```dos
refsutil salvage -FA <source volume> <working directory> <target directory> <options>
```

Realizará una fase de examen completo seguida de una fase de copia. Este modo puede tardar mucho tiempo, ya que examinará todo el volumen para detectar archivos, directorios o volúmenes recuperables.

## <a name="manual-mode-command-line-usage"></a>Uso de la línea de comandos del modo manual

### <a name="diagnose-phase-command-line-usage"></a>Uso de línea de comandos de fase de diagnóstico

```dos
refsutil salvage -D <source volume> <working directory> <options>
```

En primer lugar, intente determinar si \<source volume\> es un volumen ReFS y determine si el volumen se va a montar. Cuando un volumen no se puede montar, se determinarán los motivos. Se trata de una fase independiente.

### <a name="quick-scan-phase-command-line-usage"></a>Uso de la línea de comandos de la fase de examen rápido

```dos
refsutil salvage -QS <source volume> <working directory> <options>
```

Examen rápido \<source volume\> para cualquier archivo recuperable. Este modo se ejecuta más rápido, ya que supone que algunas estructuras críticas del volumen no están dañadas y, por lo tanto, no es necesario examinar todo el volumen para localizarlas. Esto también reduce la recuperación de archivos, directorios o volúmenes obsoletos. Los archivos detectados se registrarán en "FoundFiles. \<volume signature\> . txt "en \<working directory\> . Si la fase de exploración se detuvo previamente, la ejecución con la marca-q volverá a reanudar el análisis desde donde se quedó.

### <a name="full-scan-phase-command-line-usage"></a>Uso de la línea de comandos de la fase de examen completo

```dos
refsutil salvage -FS <source volume> <working directory> <options>
```

Examinar todos \<source volume\> los archivos recuperables. Este modo puede tardar mucho tiempo, ya que examinará todo el volumen en busca de archivos recuperables.
Los archivos detectados se registrarán en "FoundFiles. \<volume signature\> . txt "en \<working directory\> . Si la fase de exploración se detuvo previamente, al ejecutar con la marca-FS se reanudará el análisis desde donde se quedó.

### <a name="copy-phase-command-line-usage"></a>Uso de la línea de comandos de la fase de copia

```dos
refsutil salvage -C <source volume> <working directory> <target directory>
<options>
```

Copie todos los archivos descritos en "FoundFiles. \<volume signature\> . txt "a \<target
directory\> . Si la fase de examen se detiene demasiado pronto, "FoundFiles. \<volume
signature\> .. txt "puede que no se haya escrito todavía y que no se copie ningún archivo en \<target directory\> .

### <a name="copy-phase-with-list-command-line-usage"></a>Copiar fase con el uso de la línea de comandos de lista

```dos
refsutil salvage -SL <source volume> <working directory> <target
directory> <file list> <options>
```

Copie todos los archivos de \<file list\> de \<source volume\> a \<target
directory\> . Los archivos de \<file list\> deben haber sido identificados por primera vez por la fase de examen, ya que no es necesario ejecutar el examen hasta su finalización. \<file list\>se puede generar mediante la copia de "FoundFiles. \<volume signature\> . txt "en un archivo nuevo, quitando las líneas que hacen referencia a los archivos que no deben restaurarse y conservando los archivos que deben restaurarse. El cmdlet Select-String de PowerShell puede ser útil para filtrar "FoundFiles. \<volume signature\> .. txt "para incluir solo las rutas de acceso, extensiones o nombres de archivo deseados.

### <a name="copy-phase-with-interactive-console"></a>Copia de la fase con la consola interactiva

```dos
refsutil salvage -IC <source volume> <working directory> <options>
```

Recupere archivos en una consola interactiva para usuarios avanzados. Este modo también requiere archivos generados en cualquiera de las fases de examen.

## <a name="parameters"></a>Parámetros

| Parámetro             | Descripción                                                                     |
| --------------------- | --------------------------------------------------------------------------------------- |
| \<source volume\>     | Volumen de ReFS que se va a procesar. Letra de unidad en el formato "L:", o una ruta de acceso al punto de montaje del volumen.           |
| \<working directory\> | Ubicación para almacenar la información y los registros temporales. **No** debe estar ubicado en \<source volume\> .  |
| \<target directory\>  | Ubicación donde se copiarán los archivos identificados. **No** debe estar ubicado en \<source volume\> . |
| \- m         | Recupere todos los archivos posibles, incluidos los eliminados. Esta opción se omitirá en refsutil salvage v1. ADVERTENCIA: no solo tardará más tiempo en ejecutarse, lo que puede dar lugar a resultados inesperados. |
| \-v         | Modo detallado                                                                                           |
| \-x1         | Fuerce el desmontaje del volumen en primer lugar, si es necesario. Todos los identificadores abiertos en el volumen no serían válidos. Por ejemplo: refsutil salvage-QA R: N: \\ Working n: \\ Data-x                                  |
