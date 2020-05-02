---
title: cipher
description: Tema de referencia del comando Cipher, que muestra o modifica el cifrado de directorios y archivos en volúmenes NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b07a0231b6cff6be9533136bf35ef013c854c2e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713399"
---
# <a name="cipher"></a>cipher

Muestra o cambia el cifrado de directorios y archivos en volúmenes NTFS. Si se usa sin parámetros, **Cipher** muestra el estado de cifrado del directorio actual y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
cipher [/e | /d | /c] [/s:<directory>] [/b] [/h] [pathname [...]]
cipher /k
cipher /r:<filename> [/smartcard]
cipher /u [/n]
cipher /w:<directory>
cipher /x[:efsfile] [filename]
cipher /y
cipher /adduser [/certhash:<hash> | /certfile:<filename>] [/s:directory] [/b] [/h] [pathname [...]]
cipher /removeuser /certhash:<hash> [/s:<directory>] [/b] [/h] [<pathname> [...]]
cipher /rekey [pathname [...]]
```

### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| /b | Se anula si se produce un error. De forma predeterminada, el **cifrado** continúa ejecutándose incluso si se producen errores. |
| /C | Muestra información sobre el archivo cifrado. |
| /d | Descifra los archivos o directorios especificados. |
| /e | Cifra los archivos o directorios especificados. Los directorios se marcan para que se cifren los archivos que se agregan después. |
| /h | Muestra archivos con atributos ocultos o del sistema. De forma predeterminada, estos archivos no están cifrados o descifrados. |
| /k | Crea un nuevo certificado y una clave para usarlos con archivos Sistema de cifrado de archivos (EFS). Si se especifica el parámetro **/k** , se omiten todos los demás parámetros. |
| /r:`<filename>` [/SmartCard] | Genera una clave y un certificado del agente de recuperación de EFS y los escribe en un archivo. pfx (que contiene el certificado y la clave privada) y un archivo. cer (que solo contiene el certificado). Si se especifica **/SmartCard** , escribe la clave de recuperación y el certificado en una tarjeta inteligente, y no se genera ningún archivo. pfx. |
| modificado`<directory>` | Realiza la operación especificada en todos los subdirectorios del *directorio*especificado. |
| /u [/n] |  Busca todos los archivos cifrados en las unidades locales. Si se usa con el parámetro **/n** , no se realiza ninguna actualización. Si se usa sin **/n**, **/u** compara la clave de cifrado de archivos del usuario o la clave del agente de recuperación con las actuales, y las actualiza si han cambiado. Este parámetro solo funciona con **/n**. |
| /w`<directory>` | Quita los datos del espacio en disco no utilizado disponible en todo el volumen. Si usa el parámetro **/w** , se omiten todos los demás parámetros. El directorio especificado se puede ubicar en cualquier parte de un volumen local. Si es un punto de montaje o apunta a un directorio de otro volumen, se quitan los datos de ese volumen. |
| /x [: efsfile] [`<FileName>`] | Realiza una copia de seguridad del certificado EFS y de las claves en el nombre de archivo especificado. Si se usa con **: efsfile**, **/x** realiza una copia de seguridad de los certificados del usuario que se usaron para cifrar el archivo. De lo contrario, se realiza una copia de seguridad del certificado EFS y las claves actuales del usuario. |
| /y | Muestra la miniatura del certificado EFS actual en el equipo local. |
| /adduser [/certhash:`<hash>` | /CERTFILE:`<filename>`] |
| /rekey | Actualiza los archivos cifrados especificados para usar la clave EFS configurada actualmente. |
| /removeuser /certhash:`<hash>` | Quita un usuario de los archivos especificados. El *hash* proporcionado para **/certhash** debe ser el hash SHA1 del certificado que se va a quitar. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="remarks"></a>Observaciones

- Si el directorio principal no está cifrado, se puede descifrar un archivo cifrado cuando se modifica. Por lo tanto, al cifrar un archivo, también debe cifrar el directorio principal.

- Un administrador puede Agregar el contenido de un archivo. cer a la Directiva de recuperación de EFS para crear el agente de recuperación para los usuarios y, a continuación, importar el archivo. pfx para recuperar archivos individuales.

- Puede usar varios nombres de directorio y caracteres comodín.

- Debe colocar espacios entre varios parámetros.

## <a name="examples"></a>Ejemplos

Para mostrar el estado de cifrado de cada uno de los archivos y subdirectorios del directorio actual, escriba:

```
cipher
```

Los archivos y directorios cifrados se marcan con un **E**. Los archivos y directorios sin cifrar se marcan con un **U**. Por ejemplo, el siguiente resultado indica que el directorio actual y todo su contenido no están cifrados actualmente:

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```

Para habilitar el cifrado en el directorio privado usado en el ejemplo anterior, escriba:

```
cipher /e private
```

Se muestra el resultado siguiente:

```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```

El comando **Cipher** muestra el siguiente resultado:

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```

Donde el directorio **privado** ahora está marcado como cifrado.

### <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
