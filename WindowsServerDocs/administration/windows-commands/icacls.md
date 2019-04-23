---
title: icacls
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 20b2150b1135467cce43ae23bfdc275a5da22141
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852646"
---
# <a name="icacls"></a>icacls



Muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados y aplica las DACL almacenadas a los archivos de los directorios especificados.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<FileName>|Especifica el archivo que se va a mostrar la DACL.|
|\<Directory>|Especifica el directorio que se va a mostrar la DACL.|
|/t|Realiza la operación en todos los archivos especificados en el directorio actual y sus subdirectorios.|
|/c|Continúa la operación a pesar de los errores de archivo. Todavía se mostrarán mensajes de error.|
|/l|Realiza la operación en un vínculo simbólico frente a su destino.|
|/q|Suprime los mensajes de confirmación.|
|[/save \<ACLfile> [/t] [/c] [/l] [/q]]|Almacena las DACL para todos los archivos coincidentes en *ACLfile* para su uso posterior con **/restore**.|
|[/setowner \<Username> [/t] [/c] [/l] [/q]]|Cambia el propietario de todos los archivos coincidentes para el usuario especificado.|
|[/findSID \<Sid> [/t] [/c] [/l] [/q]]|Busca todos los archivos coincidentes que contienen una DACL mencionar explícitamente el identificador de seguridad especificado (SID).|
|[/verify [/t] [/c] [/l] [/q]]|Busca todos los archivos con las ACL no canónica o tienen longitudes incoherentes con un número de ACE (entrada de control de acceso).|
|[/reset [/t] [/c] [/l] [/q]]|Reemplaza ACL no tiene valor predeterminado heredan las ACL para todos los archivos coincidentes.|
|[/grant[:r] \<Sid>:<Perm>[...]]|Concede especifica derechos de acceso de usuario. Permisos de reemplazar los permisos previamente concedidos explícitos.</br>Sin **: r**, se agregan permisos a cualquier previamente concedidos permisos explícitos.|
|[/deny \<Sid>:<Perm>[...]]|Deniega explícitamente derechos de acceso de usuario especificado. Explícita Denegar ACE se agrega para los permisos indicados y se quitan los mismos permisos en cualquier concesión explícita.|
|[/remove[:g\|:d]] \<Sid>[...]] [/t] [/c] [/l] [/q]|Quita todas las repeticiones del SID especificada de la DACL.</br>**: g** quita todas las apariciones de los derechos concedidos para el SID especificado.</br>**: d.** quita todas las apariciones de denegado derechos para el SID especificado.|
|[/setintegritylevel [(CI)(OI)]\<Level>:<Policy>[...]]|Agrega explícitamente una ACE de integridad para todos los archivos coincidentes. *Nivel* se especifica como:</br>-   **L**[ow]</br>-   **M**[edia]</br>-   **H**[tercer cuartil]</br>Las opciones de herencia para la integridad ACE pueden preceder el nivel y se aplican solo a los directorios.|
|[/substitute \<SidOld> <SidNew> [...]]|Reemplaza un SID existente (*SidOld*) con un SID nuevo (*SidNew*). Requiere el *Directory* parámetro.|
|/restore \<ACLfile> [/c] [/l] [/q]|Aplica las DACL almacenadas desde *ACLfile* a los archivos del directorio especificado. Requiere el *Directory* parámetro.|
|/inheritancelevel:[e\|d\|r]|Establece el nivel de herencia: <br>  **e** -permite enheritance <br>**d.** : deshabilita la herencia y copia las ACE <br>**r** -quita todos los hereda las ACE

## <a name="remarks"></a>Comentarios

-   SID pueden estar en cualquiera de las formas en el nombre descriptivo o numéricos. Si usa un formato numérico, colocará el carácter comodín **&#42;** al principio del SID.
-   **Icacls** conserva el orden canónico de las entradas ACE como:  
    -   Denegaciones explícitas
    -   Permisos explícitos concedidos
    -   Denegaciones heredados
    -   Concede heredado
-   *Perm* es una máscara de permisos que se puede especificar en una de las formas siguientes:  
    -   Una secuencia de permisos sencillas:

        **F** (acceso completo)

        **M** (acceso de modificación)

        **RX** (lectura y acceso de ejecución)

        **R** (acceso de solo lectura)

        **W** (acceso de solo escritura)
    -   Una lista separada por comas entre paréntesis de derechos específicos:

        **D.** (eliminar)

        **RC** (control de lectura)

        **WDAC** (Escribir DAC)

        **WO** (propietario de escritura)

        **S** (sincronizar)

        **AS** (acceso a la seguridad del sistema)

        **MA** (máximo permitido)

        **GR** (lectura genérica)

        **Puerta de enlace** (escritura genérica)

        **GE** (ejecución genérica)

        **GA** (todo genérico)

        **Escritorio remoto** (leer el directorio de datos o lista)

        **WD** (escribir el archivo de datos o agregar)

        **AD** (anexar datos o Agregar subdirectorio)

        **REA** (atributos extendidos de lectura)

        **El término** (atributos extendidos de escritura)

        **X** (execute/traverse)

        **Controlador de dominio** (eliminar secundario)

        **RA** (atributos de lectura)

        **WA** (escribir atributos)
-   Derechos de herencia pueden preceder cualquiera *Perm* formulario y se aplican solo a los directorios:

    **(OI)** : heredar de objeto

    **(CI)** : heredar de contenedor

    **(IO)** : sólo heredar

    **(NP)** : no se propagan heredar

## <a name="BKMK_examples"></a>Ejemplos

Para guardar las DACL para todos los archivos del directorio C:\Windows y sus subdirectorios en el archivo ACLFile, escriba:
```
icacls c:\windows\* /save aclfile /t
```
Para restaurar las DACL para todos los archivos de ACLFile que existe en el directorio C:\Windows y sus subdirectorios, escriba:
```
icacls c:\windows\ /restore aclfile
```
Para conceder al usuario eliminar User1 y DAC escribir permisos en un archivo denominado "Test1", escriba:
```
icacls test1 /grant User1:(d,wdac)
```
Para conceder al usuario definido en los permisos Delete SID S-1-1-0 y escribir DAC a un archivo, denominado "Test2", escriba:
```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
