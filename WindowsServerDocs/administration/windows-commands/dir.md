---
title: dir
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37fdcd1f60281eedc4faa9a14e18410b1215b685
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811212"
---
# <a name="dir"></a>dir



Muestra una lista de archivos y subdirectorios de un directorio. Si se utiliza sin parámetros, **dir** muestra la etiqueta de volumen y el número de serie, seguido de una lista de directorios y archivos en el disco (incluidos sus nombres y la fecha y hora, cada uno se modificó por última vez) del disco. Para los archivos, **dir** muestra la extensión de nombre y el tamaño en bytes. **Dir** también muestra el número total de archivos y directorios que se muestran, su tamaño acumulado y el espacio libre (en bytes) del disco.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica la unidad y directorio para el que desea ver una lista.|
|[\<FileName>]|Especifica un archivo concreto o un grupo de archivos para el que desea ver una lista.|
|/p|Muestra una pantalla de la lista a la vez. Para ver la pantalla siguiente, presione cualquier tecla del teclado.|
|/q|Muestra información de la propiedad de archivo.|
|/w|Muestra la lista en formato ancho, con los nombres de archivo como máximo cinco o nombres de directorio en cada línea.|
|/d|Muestra la lista en el mismo formato que **/w**, pero los archivos se ordenan por columna.|
|/ a [[:]\<atributos >]|Muestra solo los nombres de los directorios y archivos con los atributos que especifique. Si se omite **/a**, **dir** muestra los nombres de todos los archivos excepto ocultos y los archivos del sistema. Si usas **/a** sin especificar *atributos*, **dir** muestra los nombres de todos los archivos, incluidos los ocultos y los archivos del sistema.</br>En la lista siguiente describe cada uno de los valores que puede usar para *atributos*. Uso de dos puntos (:) es opcional. Utilice cualquier combinación de estos valores y no separe los valores con espacios.</br>**d.** directorios</br>**h** archivos ocultos</br>**s** archivos del sistema</br>**l** puntos de reanálisis</br>**r** archivos de solo lectura</br>**un** archivos listos para archivar</br>**i** no archivos indizados del contenido</br>**-** Prefijo de lo que significa "no"|
|e/s [[:]\<SortOrder >]|Ordena el resultado según *SortOrder*, que puede ser cualquier combinación de los siguientes valores:</br>**n** por su nombre (por orden alfabético)</br>**e** por extensión (por orden alfabético)</br>**g** primero el grupo de directorios</br>**s** por tamaño (más pequeña primero)</br>**d.** por fecha y hora (más antiguo primero)</br>**-** Prefijo para invertir orden</br>Nota: Uso de dos puntos es opcional. Varios valores se procesan en el orden en que se enumeren. No separe varios valores con espacios.</br>Si *SortOrder* no se especifica, **dir /o** enumera los directorios en orden alfabético, seguida de los archivos, que también se ordenan en orden alfabético.|
|/t[[:]\<TimeField>]|Especifica qué campo de hora para mostrar o usar para la ordenación. En la lista siguiente describe cada uno de los valores que puede utilizar para *hora*:</br>**c** creación</br>**un** último acceso</br>**w** escribió por última vez|
|/s|Enumera todas las apariciones del nombre de archivo especificado en el directorio especificado y todos los subdirectorios.|
|/b|Muestra una lista de directorios y archivos, no hay información adicional con la reconstrucción. **/b** invalida **/w**.|
|/l|Muestra sin ordenar los nombres de directorio y los nombres de archivo en minúsculas.|
|/n|Muestra una lista en formato largo con los nombres de archivo en el extremo derecho de la pantalla.|
|/x|Muestra los nombres cortos generados para los nombres de archivo no tiene el formato 8.3. La presentación es el mismo que la presentación de **/n**, pero el nombre corto se inserta delante del nombre largo.|
|/c|Muestra el separador de miles en tamaños de archivo. Este es el comportamiento predeterminado. Use **/c** para ocultar separadores.|
|/4|Muestra los años en formato de cuatro dígitos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Para usar varios *FileName* parámetros, separe cada nombre de archivo con un espacio, coma o punto y coma.
- Puede usar caracteres comodín ( **&#42;** o **?** ), para representar uno o más caracteres de un nombre de archivo y mostrar un subconjunto de archivos o subdirectorios.

  **Asterisco (\*):** Usar el asterisco como sustituto de cualquier cadena de caracteres, por ejemplo:  
  - **dir \*.txt** muestra todos los archivos en el directorio actual con las extensiones que comienzan con .txt, por ejemplo, .txt, .txt1, .txt_old.
  - **dir leer\*.txt** muestra todos los archivos en el directorio actual que comienzan con "lectura" y con las extensiones que comienzan con .txt, como .txt, .txt1 o .txt_old.
  - **dir leer\*.\\** * enumera todos los archivos en el directorio actual que comienzan con "lectura" con cualquier extensión.

  El carácter comodín de asterisco siempre utiliza la asignación de nombres de archivo cortos, por lo que podría obtener resultados inesperados. Por ejemplo, el directorio siguiente contiene dos archivos (t.txt2 y t97.txt): 
 
  ```
  C:\test>dir /x
  Volume in drive C has no label.
  Volume Serial Number is B86A-EF32
    
  Directory of C:\test
    
  11/30/2004  01:40 PM <DIR>  .
  11/30/2004  01:40 PM <DIR> ..
  11/30/2004  11:05 AM 0 T97B4~1.TXT t.txt2
  11/30/2004  01:16 PM 0 t97.txt
  ```  

  Es de esperar ese tiempo escribiendo **dir t97\\** * devolvería el t97.txt de archivo. Sin embargo, escribir **dir t97\\** * devuelve ambos archivos, porque el carácter comodín de asterisco coincide con el t.txt2 de archivo a t97.txt mediante su asignación de nombre corto T97B4~1.TXT. De forma similar, escribiendo **del t97\\** * eliminaría ambos archivos.

  **Signo de interrogación (?):** Utilice el signo de interrogación como sustituto de un único carácter en un nombre. Por ejemplo, si escribe **dir leer???. txt** enumera los archivos en el directorio actual con la extensión .txt que comienzan con "lectura" y van seguidos por hasta tres caracteres. Esto incluye Read.txt, Read1.txt, Read12.txt, Read123.txt y Readme1.txt, pero no Readme12.txt.
- Especificar atributos de visualización del archivo

  Si usas **/a** con más de un valor en *atributos*, **dir** muestra los nombres de sólo los archivos con todos los atributos especificados. Por ejemplo, si usa **/a** con **r** y **-h** como atributos (mediante el uso **/ a: r-h** o **/ar** ), **dir** mostrará solo los nombres de los archivos de solo lectura que no están ocultos.
- Especificar la ordenación de nombre de archivo

  Si especifica más de una *SortOrder* valor, **dir** ordena los nombres de archivo por el primer criterio, después por el segundo criterio y así sucesivamente. Por ejemplo, si usa **/o** con el **e** y **-s** valores para *SortOrder* (mediante el uso **oe-s**o **/oe-s**), **dir** ordena los nombres de directorios y archivos por extensión, la más grande en primer lugar y, a continuación, muestra el resultado final. La ordenación alfabética por extensión hace sin extensiones que aparecen en primer lugar, los nombres de archivo, a continuación, los nombres de directorio y, a continuación, los nombres de archivo con extensión.
- Mediante canalizaciones y los símbolos de redirección

  Cuando se usa el símbolo de redirección ( **>** ) para enviar **dir** en un archivo o una canalización de salida ( **|** ) para enviar **dir**de salida a otro comando, use **/a:-d** y **/b** para enumerar solo los nombres de archivo. Puede usar *FileName* con **/b** y **/s** para especificar que **dir** consiste en buscar el directorio actual y sus subdirectorios para todos los archivos los nombres que coinciden con *FileName*. **Dir** muestra solo la letra de unidad, nombre de directorio, nombre de archivo y extensión de nombre de archivo (una ruta de acceso por línea), para cada archivo de nombre busca. Antes de usar una canalización para enviar **dir** de salida a otro comando, debe establecer la TEMP variable de entorno en el archivo Autoexec.nt.
- El **dir** comando, con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="examples"></a>Ejemplos

Para mostrar todos los directorios uno tras otro, en orden alfabético, en formato ancho y poner en pausa después de cada pantalla, asegúrese de que el directorio raíz es el directorio actual y, a continuación, escriba:

```
dir /s/w/o/p
```

**Dir** muestra el directorio raíz, los subdirectorios y los archivos en el directorio raíz, incluidas las extensiones. A continuación, **dir** se enumeran los nombres de subdirectorio y los nombres de archivo en cada subdirectorio en el árbol.

Para modificar el ejemplo anterior para que **dir** muestra los nombres de archivo y extensiones, pero omite los nombres de directorio, escriba:

```
dir /s/w/o/p/a:-d
```

Para imprimir una lista de directorios, escriba:

```
dir > prn
```

Al especificar **prn**, la lista de directorios se envía a la impresora que está conectada al puerto LPT1. Si la impresora está conectada a un puerto diferente, debe reemplazar **prn** con el nombre del puerto correcto.

También puede redirigir la salida de la **dir** a un archivo reemplazando **prn** con un nombre de archivo. También puede escribir una ruta de acceso. Por ejemplo, a direct **dir** de salida para el archivo dir.doc en el directorio de registros, escriba:

```
dir > \records\dir.doc
```

Si no existe el archivo dir.doc, **dir** lo crea, a menos que no existe el directorio de registros. En ese caso, aparece el siguiente mensaje:

`File creation error`

Para mostrar una lista de todos los nombres de archivo con la extensión .txt en todos los directorios en la unidad C, escriba:

```
dir c:\*.txt /w/o/s/p
```

**Dir** muestra, en formato ancho, nombres de una lista ordenada alfabéticamente de los archivos coincidentes en cada directorio y pone en pausa cada vez que se rellena de la pantalla hasta que presione cualquier tecla para continuar.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)