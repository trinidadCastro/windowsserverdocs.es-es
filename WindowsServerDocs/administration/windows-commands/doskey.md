---
title: doskey
description: Artículo de referencia para el comando Doskey y Doskey.exe, que recupera comandos de la línea de comandos previamente especificados, edita líneas de comandos y crea macros.
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f945c0b73509e0a936bf4de1cae9bb721b77e5c3
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890765"
---
# <a name="doskey"></a>doskey

Llama a Doskey.exe, que recupera comandos de la línea de comandos previamente especificados, edita líneas de comandos y crea macros.

## <a name="syntax"></a>Sintaxis

```
doskey [/reinstall] [/listsize=<size>] [/macros:[all | <exename>] [/history] [/insert | /overstrike] [/exename=<exename>] [/macrofile=<filename>] [<macroname>=[<text>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /REINSTALL | Instala una nueva copia de Doskey.exe y borra el búfer del historial de comandos. |
| /LISTSIZE =`<size>` | Especifica el número máximo de comandos en el búfer del historial. |
| /macros | Muestra una lista de todas las macros de **doskey** . Puede utilizar el símbolo de redirección ( `>` ) con **/macros** para redirigir la lista a un archivo. Puede abreviar **/macros** para **/m**. |
| /macros: ALL | Muestra macros de **doskey** para todos los ejecutables. |
| /macros`<exename>` | Muestra las macros de **doskey** para el ejecutable especificado por *EXENAME*. |
| /History | Muestra todos los comandos que se almacenan en memoria. Puede utilizar el símbolo de redirección ( `>` ) con **/History** para redirigir la lista a un archivo. Se puede abreviar **/History** como **/h**. |
| /insert | Especifica que el nuevo texto que escriba se insertará en el texto anterior. |
| /overstrike | Especifica que el nuevo texto sobrescribe el texto anterior. |
| /EXENAME =`<exename>` | Especifica el programa (es decir, ejecutable) en el que se ejecuta la macro **doskey** . |
| /MACROFILE =`<filename>` | Especifica un archivo que contiene las macros que desea instalar. |
| `<macroname>`=[`<text>`] | Crea una macro que lleva a cabo los comandos especificados por el *texto*. *Nombremacro* especifica el nombre que desea asignar a la macro. *Texto* especifica los comandos que desea registrar. Si el *texto* se deja en blanco, *nombremacro* se borra de los comandos asignados. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Ciertos programas interactivos basados en caracteres, como depuradores de programas o programas de transferencia de archivos (FTP), utilizan automáticamente Doskey.exe. Para usar Doskey.exe, un programa debe ser un proceso de consola y usar una entrada almacenada en búfer. Las asignaciones de clave de programa invalidan las asignaciones de teclas de **doskey** . Por ejemplo, si el programa utiliza la tecla F7 para una función, no se puede obtener un historial de comandos de **doskey** en una ventana emergente.

- Puede usar Doskey.exe para editar la línea de comandos actual, pero no puede usar las opciones de línea de comandos desde el símbolo del sistema de un programa. Debe ejecutar las opciones de línea de comandos de **doskey** antes de iniciar un programa. Si usa Doskey.exe dentro de un programa, las asignaciones de clave de ese programa tienen prioridad y algunas Doskey.exe claves de edición podrían no funcionar.

- Con Doskey.exe, puede mantener un historial de comandos para cada programa que inicie o repita. Puede editar los comandos anteriores en el símbolo del sistema e iniciar las macros de **doskey** creadas para el programa. Si sale y después reinicia un programa desde la misma ventana del símbolo del sistema, está disponible el historial de comandos de la sesión de programa anterior.

- Para recuperar un comando, puede utilizar cualquiera de las siguientes claves después de iniciar Doskey.exe:

  | Clave | Descripción |
  | --- | ----------- |
  | FLECHA ARRIBA | Recupera el comando que usó antes del que se muestra. |
  | FLECHA ABAJO | Recupera el comando que usó después del que se muestra. |
  | RE PÁG | Recupera el primer comando que usó en la sesión actual. |
  | AV PÁG | Recupera el comando más reciente que usó en la sesión actual. |

- En la tabla siguiente se enumeran las claves de edición de **doskey** y sus funciones:

  | Tecla o combinación de teclas | Descripción |
  | ---------------------- | ----------- |
  | FLECHA IZQUIERDA | Mueve el punto de inserción un carácter hacia atrás. |
  | FLECHA DERECHA | Mueve el punto de inserción hacia delante un carácter. |
  | CTRL+FLECHA IZQUIERDA | Mueve el punto de inserción una palabra hacia atrás. |
  | CTRL+FLECHA DERECHA | Mueve el punto de inserción hacia delante una palabra. |
  | INICIO | Mueve el punto de inserción al principio de la línea. |
  | FIN | Mueve el punto de inserción al final de la línea. |
  | ESC | Borra el comando de la pantalla. |
  | F1 | Copia un carácter de una columna de la plantilla en la misma columna de la ventana del símbolo del sistema. (La plantilla es un búfer de memoria que contiene el último comando que escribió). |
  | F2 | Busca hacia delante en la plantilla la siguiente tecla que escriba después de presionar F2. Doskey.exe inserta el texto de la plantilla, hasta el carácter especificado, pero sin incluirlo. |
  | F3 | Copia el resto de la plantilla en la línea de comandos. Doskey.exe comienza a copiar los caracteres de la posición en la plantilla que corresponde a la posición indicada por el punto de inserción en la línea de comandos. |
  | F4 | Elimina todos los caracteres de la posición actual del punto de inserción hasta la siguiente aparición del carácter que se escribe después de presionar F4, sin incluirlo. |
  | F5 | Copia la plantilla en la línea de comandos actual. |
  | F6 | Coloca un carácter de fin de archivo (CTRL + Z) en la posición del punto de inserción actual. |
  | F7 | Muestra (en un cuadro de diálogo) todos los comandos para este programa que se almacenan en memoria. Use la tecla flecha arriba y la tecla flecha abajo para seleccionar el comando que desee y presione Entrar para ejecutar el comando. También puede anotar el número secuencial delante del comando y usar este número junto con la tecla F9. |
  | ALT+F7 | Elimina todos los comandos almacenados en memoria para el búfer de historial actual. |
  | F8 | Muestra todos los comandos del búfer de historial que comienzan con los caracteres del comando actual. |
  | F9 | Solicita un número de comando de búfer de historial y, a continuación, muestra el comando asociado al número que especifique. Presione ENTRAR para ejecutar el comando. Para mostrar todos los números y sus comandos asociados, presione F7. |
  | ALT+F10 | Elimina todas las definiciones de macro. |

- Si presiona la tecla insertar, puede escribir texto en la línea de comandos de **doskey** en medio de texto existente sin reemplazar el texto. Sin embargo, después de presionar entrar, Doskey.exe devuelve el teclado para **reemplazar** el modo. Debe presionar insertar de nuevo para volver al modo de **inserción** .

- El punto de inserción cambia de forma cuando se usa la tecla insertar para cambiar de un modo a otro.

- Si desea personalizar cómo funciona Doskey.exe con un programa y crear macros de **doskey** para ese programa, puede crear un programa por lotes que modifique Doskey.exe e inicie el programa.

- Puede usar Doskey.exe para crear macros que realicen uno o más comandos. En la tabla siguiente se enumeran los caracteres especiales que se pueden usar para controlar las operaciones de comandos cuando se define una macro.

  | Carácter | Descripción |
  |---------- | ----------- |
  | `$G` o `$g` | Redirige la salida. Use cualquiera de estos caracteres especiales para enviar la salida a un dispositivo o un archivo en lugar de a la pantalla. Este carácter es equivalente al símbolo de redirección de Output ( `>` ). |
  | `$G$G` o `$g$g` | Anexa la salida al final de un archivo. Use cualquiera de estos dos caracteres dobles para anexar la salida a un archivo existente en lugar de reemplazar los datos del archivo. Estos caracteres dobles son equivalentes al símbolo de redirección de anexado para la salida ( `>>` ). |
  | `$L` o `$l` | Redirige la entrada. Use cualquiera de estos caracteres especiales para leer la entrada de un dispositivo o un archivo en lugar de hacerlo desde el teclado. Este carácter es equivalente al símbolo de redirección para la entrada ( `<` ). |
  | `$B` o `$b` | Envía la salida de la macro a un comando. Estos caracteres especiales son equivalentes al uso de la canalización `(` y `*` . |
  | `$T` o `$t` | Separa los comandos. Use cualquiera de estos caracteres especiales para separar comandos cuando cree macros o comandos de tipo en la línea de comandos de **doskey** . Estos caracteres especiales son equivalentes al uso del símbolo de y comercial ( `&` ) en una línea de comandos. |
  | `$$` | Especifica el carácter de signo de dólar ( `$` ). |
  | `$1`por`$9` | Represente la información de línea de comandos que desee especificar al ejecutar la macro. Los caracteres especiales `$1` a través de `$9` son parámetros de lote que permiten usar datos diferentes en la línea de comandos cada vez que se ejecuta la macro. El `$1` carácter de un comando **doskey** es similar al `%1` de un programa por lotes. |
  | `$*` | Representa toda la información de línea de comandos que desea especificar al escribir el nombre de la macro. El carácter especial `$*` es un parámetro reemplazable que es similar a los parámetros de lote `$1` a través de `$9` , con una diferencia importante: todo lo que escriba en la línea de comandos después de que el nombre de la macro se sustituya por el `$*` de la macro. |

- Para ejecutar una macro, escriba el nombre de la macro en el símbolo del sistema, comenzando en la primera posición. Si la macro se definió con `$*` o con cualquiera de los parámetros del lote `$1` a través de `$9` , use un espacio para separar los parámetros. No se puede ejecutar una macro **doskey** desde un programa por lotes.

- Si siempre usa un comando determinado con opciones específicas de la línea de comandos, puede crear una macro que tenga el mismo nombre que el comando. Para especificar si desea ejecutar la macro o el comando, siga estas instrucciones:

  - Para ejecutar la macro, escriba el nombre de la macro en el símbolo del sistema. No agregue un espacio delante del nombre de la macro.

  - Para ejecutar el comando, inserte uno o más espacios en el símbolo del sistema y, a continuación, escriba el nombre del comando.

### <a name="examples"></a>Ejemplos

Las opciones de línea de comandos **/macros** y **/History** son útiles para crear programas de batch para guardar macros y comandos. Por ejemplo, para almacenar todas las macros de **doskey** actuales, escriba:

```
doskey /macros > macinit
```

Para usar las macros almacenadas en Macinit, escriba:

```
doskey /macrofile=macinit
```

Para crear un programa de Batch denominado Tmp.bat que contenga los comandos usados recientemente, escriba:

```
doskey /history> tmp.bat
```

Para definir una macro con varios comandos, use `$t` para separar comandos, como se indica a continuación:

```
doskey tx=cd temp$tdir/w $*
```

En el ejemplo anterior, la macro TX cambia el directorio actual a Temp y, a continuación, muestra una lista de directorios en formato de visualización ancho. Puede usar `$*` al final de la macro para anexar otras opciones de la línea de comandos a **dir** al ejecutar la opción TX.

La siguiente macro usa un parámetro batch para un nuevo nombre de directorio:

```
doskey mc=md $1$tcd $1
```

La macro crea un nuevo directorio y, a continuación, cambia al directorio nuevo desde el directorio actual.

Para usar la macro anterior para crear y cambiar a un directorio denominado *books*, escriba:

```
mc books
```

Para crear una macro **doskey** para un programa llamado *Ftp.exe*, incluya **/EXENAME** como se indica a continuación:

```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye
```

Para usar la macro anterior, inicie FTP. En el símbolo del sistema FTP, escriba:

```
go
```

FTP ejecuta los comandos **Open**, **mget**y **bye** .

Para crear una macro que dé formato a un disco de forma rápida e incondicional, escriba:

```
doskey qf=format $1 /q /u
```

Para formatear rápida y incondicionalmente un disco en la unidad A, escriba:

```
qf a:
```

Para eliminar una macro denominada *Vlist*, escriba:

```
doskey vlist =
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
