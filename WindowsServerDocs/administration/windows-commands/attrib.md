---
title: attrib
description: Tema de los comandos de Windows para **attrib** -muestra, Establece o quita atributos asignados a los archivos o directorios.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f640af2c7957e43dd3f31dfa732bfc887112651
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841096"
---
# <a name="attrib"></a>attrib



Muestra, Establece o quita los atributos asignados a los archivos o directorios. Si se utiliza sin parámetros, **attrib** muestra los atributos de todos los archivos en el directorio actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|{+\|-}r|Conjuntos (**+**) o desactiva (**-**) el atributo de archivo de solo lectura.|
|{+\|-}a|Conjuntos (**+**) o desactiva (**-**) el atributo de archivo modificado.|
|{+\|-}s|Conjuntos (**+**) o desactiva (**-**) el atributo de archivo del sistema.|
|{+\|-}h|Conjuntos (**+**) o desactiva (**-**) el atributo de archivo oculto.|
|{+\|-}i|Conjuntos (**+**) o desactiva (**-**) el atributo de archivo no contenido indizado.|
|[\<Drive>:][<Path>][<FileName>]|Especifica la ubicación y el nombre del directorio, archivo o grupo de archivos para el que desea mostrar o cambiar los atributos. ¿Puede usar el **?** y **&#42;** caracteres comodín en el *FileName* parámetro para mostrar o cambiar los atributos de un grupo de archivos.|
|/s|Se aplica **attrib** y todas las opciones de línea de comandos a los archivos coincidentes en el directorio actual y todos sus subdirectorios.|
|/d|Se aplica **attrib** y las opciones de línea de comandos a los directorios.|
|/l|Se aplica **attrib** y las opciones de línea de comandos para el vínculo simbólico, en lugar de destino del vínculo simbólico.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   ¿Puede usar caracteres comodín (**?** y **&#42;**) con el *FileName* parámetro para mostrar o cambiar los atributos de un grupo de archivos.
-   Si dispone de un archivo del sistema (**s**) o Hidden (**h**) conjunto de atributos, debe borrar el atributo antes de poder cambiar cualquier otro atributo para ese archivo.
-   El atributo de archivo (**un**) marca los archivos que han cambiado desde la última vez que su copia de seguridad. Tenga en cuenta que el **xcopy** comando utiliza los atributos de archivo.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar los atributos de un archivo llamado Notic86 que se que se encuentra en el directorio actual, escriba:
```
attrib news86 
```
Para asignar el atributo de sólo lectura al archivo informe.txt, escriba:
```
attrib +r report.txt 
```
Para quitar el atributo de sólo lectura de archivos del directorio público y sus subdirectorios en un disco de la unidad B, escriba:
```
attrib -r b:\public\*.* /s 
```
Para establecer el atributo Archive de todos los archivos de la unidad A y, a continuación, desactive el atributo Archive de archivos con la extensión .bak, escriba:
```
attrib +a a:*.* & attrib -a a:*.bak 
```
