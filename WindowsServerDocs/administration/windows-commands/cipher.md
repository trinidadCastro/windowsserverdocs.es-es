---
title: cipher
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d801d6e6286e97319766c879f7289f6191cc7101
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434337"
---
# <a name="cipher"></a>cipher



Muestra o cambia el cifrado de directorios y archivos en volúmenes NTFS. Si se utiliza sin parámetros, **cifrado** muestra el estado de cifrado del directorio actual y los archivos que contiene.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
cipher [/e | /d | /c] [/s:<Directory>] [/b] [/h] [PathName [...]]
cipher /k
cipher /r:<FileName> [/smartcard]
cipher /u [/n]
cipher /w:<Directory>
cipher /x[:efsfile] [FileName]
cipher /y
cipher /adduser [/certhash:<Hash> | /certfile:<FileName>] [/s:Directory] [/b] [/h] [PathName [...]]
cipher /removeuser /certhash:<Hash> [/s:<Directory>] [/b] [/h] [<PathName> [...]]
cipher /rekey [PathName [...]]
```

## <a name="parameters"></a>Parámetros

|          Parámetros           |                                                                                                                                                   Descripción                                                                                                                                                    |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /b               |                                                                                                    Anula si se produce un error. De forma predeterminada, **cifrado** continúa ejecutándose incluso si se producen errores.                                                                                                    |
|              /c               |                                                                                                                                   Muestra información sobre el archivo cifrado.                                                                                                                                    |
|              /d               |                                                                                                                                   Descifra los archivos o directorios especificados.                                                                                                                                   |
|              /e               |                                                                                          Cifra los archivos o directorios especificados. Los directorios se marcan para que se cifrarán los archivos que se agreguen más adelante.                                                                                           |
|              /h               |                                                                                                     Muestra los archivos con oculto o atributos del sistema. De forma predeterminada, estos archivos no se cifra o descifra.                                                                                                     |
|              /k               |                                                                            Crea un nuevo certificado y una clave para su uso con archivos de sistema de archivos cifrados (EFS). Si el **/k** parámetro se especifica, se omiten todos los demás parámetros.                                                                            |
|  /r:\<FileName> [/smartcard]  |   Genera una clave del agente de recuperación de EFS y un certificado y luego escribe en un archivo .pfx (que contiene los certificados y la clave privada) y un archivo .cer (que contiene solo el certificado). Si **/smartcard** se especifica, escriben la clave de recuperación y el certificado en una tarjeta inteligente y se genera ningún archivo pfx.   |
|        / s:\<directorio >        |                                                                                                               Realiza la operación especificada en todos los subdirectorios en la instancia especificada *Directory*.                                                                                                               |
|            /u [/n]            |  Busca todos los archivos cifrados en las unidades locales. Si se usa con el **/n** parámetro, no se actualiza. Si se usa sin **/n**, **/u** compara la clave de cifrado de archivos del usuario o la clave del agente de recuperación a las ya existentes y las actualiza si han cambiado. Este parámetro solo funciona con **/n**.  |
|        / w:\<directorio >        | Quita los datos de espacio en disco sin usar disponible en todo el volumen. Si usas el **/w** parámetro, se omiten todos los demás parámetros. El directorio especificado puede encontrarse en cualquier lugar en un volumen local. Si es un montaje de punto o apunta a un directorio en otro volumen, los datos en que se ha quitado el volumen. |
|  /x[:efsfile] [\<FileName>]   |                                 Copia el certificado EFS y las claves en el nombre de archivo especificado. Si se usa con **: efsfile**, **/x** realiza copias de seguridad o los certificados del usuario que se usaron para cifrar el archivo. En caso contrario, el usuario actual del certificado EFS y claves son una copia de seguridad.                                 |
|              /y               |                                                                                                                      Muestra la miniatura de certificado EFS actual en el equipo local.                                                                                                                      |
|  /AddUser [/ certhash:\<Hash >  |                                                                                                                                              /certfile:<FileName>]                                                                                                                                               |
|            /rekey             |                                                                                                                 Actualiza los archivos cifrados especificados para usar la clave de EFS actualmente configurada.                                                                                                                 |
| /removeuser /certhash:\<Hash> |                                                                                       Quita un usuario de los archivos especificados. El *Hash* previstas **/certhash** debe ser el hash SHA1 del certificado que se va a quitar.                                                                                       |
|              /?               |                                                                                                                                       Muestra la ayuda en el símbolo del sistema.                                                                                                                                       |

## <a name="remarks"></a>Comentarios

-   Si el directorio principal no está cifrado, se pudo descifrar un archivo cifrado cuando se modifica. Por lo tanto, al cifrar un archivo, se debe cifrar el directorio primario.
-   Un administrador puede agregar el contenido de un archivo .cer para la directiva de recuperación EFS para crear al agente de recuperación para los usuarios y, a continuación, importar el archivo .pfx para recuperar archivos individuales.
-   Puede usar varios nombres de directorio y caracteres comodín.
-   Debe insertar espacios entre los distintos parámetros.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el estado de cifrado de cada uno de los archivos y subdirectorios del directorio actual, escriba:
```
cipher
```
Cifrado archivos y directorios se marcan con un **E**. Sin cifrar archivos y directorios se marcan con un **U**. Por ejemplo, el siguiente resultado indica que el directorio actual y todo su contenido está cifrado actualmente:
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```
Para habilitar el cifrado en el directorio privado utilizado en el ejemplo anterior, escriba:
```
cipher /e private
```
Muestra el siguiente resultado:
```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```
El **cifrado** comando muestra el siguiente resultado:
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```
Tenga en cuenta que el directorio privado está marcado como cifrada.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)