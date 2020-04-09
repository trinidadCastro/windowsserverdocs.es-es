---
title: icacls
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 59d10b9ed681b7e0af120798dde9f200182d67d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842268"
---
# <a name="icacls"></a>icacls

Muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados y aplica las DACL almacenadas a los archivos de los directorios especificados.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<nombre de archivo >|Especifica el archivo para el que se van a mostrar las DACL.|
|> de \<de directorio|Especifica el directorio para el que se van a mostrar las DACL.|
|/t|Realiza la operación en todos los archivos especificados en el directorio actual y en sus subdirectorios.|
|/c|Continúa la operación a pesar de los errores de archivo. Todavía se mostrarán los mensajes de error.|
|/l|Realiza la operación en un vínculo simbólico frente a su destino.|
|/q|Suprime los mensajes de operación correcta.|
|[/Save \<ACLfile > [/t] [/c] [/l] [/q]]|Almacena listas DACL para todos los archivos coincidentes en *ACLfile* para su uso posterior con **/restore**.|
|[/SetOwner \<nombreDeUsuario > [/t] [/c] [/l] [/q]]|Cambia el propietario de todos los archivos coincidentes al usuario especificado.|
|[/findSID \<SID > [/t] [/c] [/l] [/q]]|Busca todos los archivos coincidentes que contienen una DACL que menciona explícitamente el identificador de seguridad (SID) especificado.|
|[/Verify [/t] [/c] [/l] [/q]]|Busca todos los archivos con ACL que no son canónicos o tienen longitudes incoherentes con los recuentos de ACE (entrada de control de acceso).|
|[/RESET [/t] [/c] [/l] [/q]]|Reemplaza las ACL con las ACL heredadas predeterminadas para todos los archivos coincidentes.|
|[/Grant [: r] \<SID >:<Perm>[...]]|Concede derechos de acceso de usuario especificados. Los permisos reemplazan a los permisos explícitos concedidos previamente.</br>Sin **: r**, los permisos se agregan a los permisos explícitos concedidos previamente.|
|[/deny \<SID >:<Perm>[...]]|Deniega explícitamente los derechos de acceso de usuario especificados. Se agrega una ACE de denegación explícita para los permisos indicados y se quitan los mismos permisos en cualquier concesión explícita.|
|[/Remove [: g\|:d]] \<SID > [...]] /t /c l /q|Quita todas las apariciones del SID especificado de la DACL.</br>**: g** quita todas las apariciones de los derechos concedidos al SID especificado.</br>**:d** quita todas las repeticiones de derechos denegados para el SID especificado.|
|[/setintegritylevel [(CI) (OI)] nivel de\<>:<Policy>[...]]|Agrega explícitamente una ACE de integridad a todos los archivos coincidentes. *LEVEL* se especifica como:</br>-   **L**[ostrar]</br>-   **M**[edium]</br>-   **H**[IGH]</br>Las opciones de herencia para la ACE de integridad pueden preceder al nivel y solo se aplican a los directorios.|
|[/Substitute \<SidOld > <SidNew> [...]]|Reemplaza un SID existente (*SidOld*) por un nuevo SID (*SidNew*). Requiere el parámetro *Directory* .|
|/restore \<ACLfile > [/c] [/l] [/q]|Aplica las listas DACL almacenadas de *ACLfile* a los archivos del directorio especificado. Requiere el parámetro *Directory* .|
|/InheritanceLevel: [e\|d\|r]|Establece el nivel de herencia: <br>  **e** : habilita la <br>**d** : deshabilita la herencia y copia las ACE <br>**r** -quita todas las ACE heredadas

## <a name="remarks"></a>Comentarios

-   Los SID pueden estar en formato de nombre numérico o descriptivo. Si usa un formato numérico, coloque el carácter **&#42;** comodín al principio del SID.
-   **icacls** conserva el orden canónico de las entradas ACE como:  
    -   Denegaciones explícitas
    -   Concesiones explícitas
    -   Denegaciones heredadas
    -   Concesiones heredadas
-   *Perm* es una máscara de permisos que se puede especificar en una de las siguientes formas:  
    -   Una secuencia de derechos simples:

        **F** (acceso completo)

        **M** (modificar acceso)

        **RX** (acceso de lectura y ejecución)

        **R** (acceso de solo lectura)

        **W** (acceso de solo escritura)
    -   Lista separada por comas entre paréntesis de derechos específicos:

        **D** (eliminar)

        **RC** (control de lectura)

        **WDAC** (escribir DAC)

        **WO** (escribir propietario)

        **S** (sincronizar)

        **As** (seguridad del sistema de acceso)

        **MA** (máximo permitido)

        **Gr** (lectura genérica)

        **GW** (escritura genérica)

        **GE** (ejecución genérica)

        **GA** (genérico todo)

        **Rd** (leer el directorio de datos/lista)

        **WD** (escribir datos/Agregar archivo)

        **Ad** (anexar datos/agregar subdirectorio)

        **Rea** (leer atributos extendidos)

        **Término** (escribir atributos extendidos)

        **X** (ejecutar/atravesar)

        **DC** (eliminar secundario)

        **RA** (atributos de lectura)

        **Wa** (escribir atributos)
-   Los derechos de herencia pueden preceder a un formulario *Perm* y solo se aplican a los directorios:

    **(OI)** : herencia de objeto

    **(CI)** : herencia de contenedor

    **(E/s)** : heredar solo

    **(NP)** : no propagar herencia

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
