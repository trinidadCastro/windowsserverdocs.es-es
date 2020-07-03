---
title: icacls
description: Artículo de referencia del comando icacls, que muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados y aplica las DACL almacenadas a los archivos de los directorios especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 386e008ef7095cbef8d84b33682b494d8d6c9c52
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924529"
---
# <a name="icacls"></a>icacls

Muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados y aplica las DACL almacenadas a los archivos de los directorios especificados.

> [!NOTE]
> Este comando reemplaza el [comando cacls](cacls.md)en desuso.

## <a name="syntax"></a>Sintaxis

```
icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]
icacls <directory> [/substitute <sidold> <sidnew> [...]] [/restore <aclfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<filename>` | Especifica el archivo para el que se van a mostrar las DACL. |
| `<directory>` | Especifica el directorio para el que se van a mostrar las DACL. |
| /t | Realiza la operación en todos los archivos especificados en el directorio actual y en sus subdirectorios. |
| /C | Continúa la operación a pesar de los errores de archivo. Todavía se mostrarán los mensajes de error. |
| /l | Realiza la operación en un vínculo simbólico en lugar de en su destino. |
| /q | Suprime los mensajes de operación correcta. |
| [/Save `<ACLfile>` /t /c l [/q]] | Almacena listas DACL para todos los archivos coincidentes en *ACLfile* para su uso posterior con **/restore**. |
| [/SetOwner `<username>` /t /c l [/q]] | Cambia el propietario de todos los archivos coincidentes al usuario especificado. |
| [/findsid `<sid>` /t /c l [/q]] | Busca todos los archivos coincidentes que contienen una DACL que menciona explícitamente el identificador de seguridad (SID) especificado. |
| [/Verify [/t] [/c] [/l] [/q]] | Busca todos los archivos con ACL que no son canónicos o tienen longitudes incoherentes con los recuentos de ACE (entrada de control de acceso). |
| [/RESET [/t] [/c] [/l] [/q]] | Reemplaza las ACL con las ACL heredadas predeterminadas para todos los archivos coincidentes. |
| [/Grant [: r] \<sid> : <perm> [...]] | Concede derechos de acceso de usuario especificados. Los permisos reemplazan a los permisos explícitos concedidos previamente.<p>No agregar **: r**significa que los permisos se agregan a los permisos explícitos concedidos previamente. |
| [/deny \<sid> : <perm> [...]] | Deniega explícitamente los derechos de acceso de usuario especificados. Se agrega una ACE de denegación explícita para los permisos indicados y se quitan los mismos permisos en cualquier concesión explícita. |
| [/Remove `[:g | :d]]` `<sid>` [...] /t /c l /q | Quita todas las apariciones del SID especificado de la DACL. Este comando también puede usar:<ul><li>**: g** : quita todas las apariciones de los derechos concedidos al SID especificado.</li><li>**:d** : quita todas las repeticiones de derechos denegados para el SID especificado. |
| [/setintegritylevel [(CI) (OI)] `<Level>:<Policy>` [...]] | Agrega explícitamente una ACE de integridad a todos los archivos coincidentes. El nivel se puede especificar como:<ul><li>**l** -Low</li><li>**m**-Medium</li><li>**h** : alta</li></ul>Las opciones de herencia para la ACE de integridad pueden preceder al nivel y solo se aplican a los directorios. |
| [/Substitute `<sidold> <sidnew>` [...]] | Reemplaza un SID existente (*sidold*) por un nuevo SID (*sidnew*). Requiere el uso de con el `<directory>` parámetro. |
| /restore `<ACLfile>` [/c] [/l] [/q] | Aplica las listas DACL almacenadas de `<ACLfile>` a los archivos del directorio especificado. Requiere el uso de con el `<directory>` parámetro. |
| /inheritancelevel:`[e | d | r]` | Establece el nivel de herencia, que puede ser:<ul><li>**e** : habilita la herencia</li><li>**d** : deshabilita la herencia y copia las ACE</li><li>**r** -quita todas las ACE heredadas</li></ul> |

## <a name="remarks"></a>Comentarios

- Los SID pueden estar en formato de nombre numérico o descriptivo. Si usa un formato numérico, coloque el carácter comodín **&#42;** al principio del SID.

- Este comando conserva el orden canónico de las entradas ACE como:

    - Denegaciones explícitas

    -  Concesiones explícitas

    - Denegaciones heredadas

    - Concesiones heredadas

- La `<perm>` opción es una máscara de permisos que se puede especificar en uno de los siguientes formatos:

    - Una secuencia de derechos simples:

      - Acceso total a **F**

      - **M**: modificación del acceso

      - **RX** : acceso de lectura y ejecución

      - **R** : acceso de solo lectura

      - Acceso **de solo** escritura

    - Lista separada por comas entre paréntesis de derechos específicos:

      - **D** : eliminar

      - **RC** : control de lectura

      - **WDAC** : escribir DAC

      - **WO** -escribir propietario

      - **S** -sincronizar

      - Seguridad del sistema **as** -Access

      - **MA** -máximo permitido

      - **Gr** : lectura genérica

      - **GW** : escritura genérica

      - **GE** : ejecución genérica

      - **GA** -genérico todo

      - **Rd** -leer datos/directorio de lista

      - **WD** -escribir datos/Agregar archivo

      - **Ad** -anexar datos/agregar subdirectorio

      - **Rea** -leer atributos extendidos

      - **Término** : escribir atributos extendidos

      - **X** -ejecutar/atravesar

      - **DC** : eliminar secundario

      - **RA** -leer atributos

      - **Wa** -escribir atributos

  - Los derechos de herencia pueden preceder `<perm>` a cualquier forma y solo se aplican a los directorios:

      - **(OI)** : herencia de objeto

      - **(CI)** : herencia de contenedor

      - **(E/s)** : heredar solo

      - **(NP)** : no propagar herencia

## <a name="examples"></a>Ejemplos

Para guardar las DACL de todos los archivos del directorio C:\Windows y de sus subdirectorios en el archivo ACLFile, escriba:

```
icacls c:\windows\* /save aclfile /t
```

Para restaurar las DACL de cada archivo dentro de ACLFile que exista en el directorio C:\Windows y en sus subdirectorios, escriba:

```
icacls c:\windows\ /restore aclfile
```

Para conceder al usuario user1 eliminar y escribir permisos de DAC en un archivo denominado prueba1, escriba:

```
icacls test1 /grant User1:(d,wdac)
```

Para conceder al usuario definido por SID S-1-1-0 eliminar y escribir permisos de DAC en un archivo, denominado test2, escriba:

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
