---
title: replace
description: Obtenga información acerca de cómo usar el comando Replace para reemplazar archivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4ac424154968b4f4c55664d0d20f524345b87986
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722382"
---
# <a name="replace"></a>replace



Reemplaza los archivos. Si se usa con la opción **/a** , **reemplazar** agrega nuevos archivos a un directorio en lugar de reemplazar los archivos existentes.



## <a name="syntax"></a>Sintaxis

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> unidad1:] [\<Ruta1>] \<Nombre de archivo>|Especifica la ubicación y el nombre del archivo de código fuente o del conjunto de archivos. *Filename* es obligatorio y puede incluir caracteres comodín (**&#42;** y **?**).|
|[\<Unidad2>:] [\<Ruta2>]|Especifica la ubicación del archivo de destino. No se puede especificar un nombre de archivo para los archivos que se reemplacen. Si no especifica una unidad o una ruta de acceso, **Replace** utiliza la unidad y el directorio actuales como destino.|
|/a|Agrega nuevos archivos al directorio de destino en lugar de reemplazar los archivos existentes. No puede usar esta opción de línea de comandos con la opción de línea de comandos **/s** o **/u** .|
|/p|Le pide confirmación antes de reemplazar un archivo de destino o de agregar un archivo de origen.|
|/r|Reemplaza los archivos de solo lectura y los no protegidos. Si intenta reemplazar un archivo de solo lectura, pero no especifica **/r**, se genera un error y se detiene la operación de reemplazo.|
|/w|Espera a que inserte un disco antes de que se inicie la búsqueda de archivos de código fuente. Si no especifica **/w**, **Replace** comienza a reemplazar o agregar archivos inmediatamente después de presionar entrar.|
|/s|Busca todos los subdirectorios en el directorio de destino y reemplaza los archivos coincidentes. No se puede usar **/s** con la opción de línea de comandos **/a** . El comando **Replace** no busca subdirectorios que se especifican en *ruta1*.|
|/U|Reemplaza solo los archivos del directorio de destino que sean más antiguos que los del directorio de origen. No se puede usar **/u** con la opción de línea de comandos **/a** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- Cuando **Replace** agrega o reemplaza archivos, los nombres de archivo se muestran en la pantalla. Una vez finalizado el **reemplazo** , se muestra una línea de resumen en uno de los siguientes formatos:  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- Si usa disquetes y necesita cambiar de disco durante la operación de **reemplazo** , puede especificar la opción de línea de comandos **/w** para que **Replace** espere a que cambie los discos.
- No se puede usar **reemplazar** para actualizar archivos ocultos o archivos del sistema.
- En la tabla siguiente se muestra cada código de salida y una breve descripción de su significado:  
  |Código de salida|Descripción|
  |---------|-----------|
  |0|El comando **Replace** ha reemplazado o agregado los archivos correctamente.|
  |1|El comando **Replace** encontró una versión incorrecta de ms-dos.|
  |2|El comando **Replace** no encontró los archivos de origen.|
  |3|El comando **Replace** no encontró la ruta de acceso de origen o de destino.|
  |5|El usuario no tiene acceso a los archivos que desea reemplazar.|
  |8|No hay suficiente memoria del sistema para ejecutar el comando.|
  |11|El usuario usó la sintaxis incorrecta en la línea de comandos.|

> [!NOTE]
> Puede usar el parámetro ERRORLEVEL en la línea de comandos **If** en un programa por lotes para procesar códigos de salida devueltos por **Replace**.

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos

Para actualizar todas las versiones de un archivo denominado PHONES. CLI (que aparecen en varios directorios de la unidad C), con la versión más reciente del archivo PHONES. CLI de un disquete en la unidad A, escriba:

`replace a:\phones.cli c:\ /s`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)