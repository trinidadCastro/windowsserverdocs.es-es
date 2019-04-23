---
title: tpmvscmgr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cfc15b7c089c9b6ad4d9a267b951373e63a63a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888596"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



La herramienta de línea de comandos Tpmvscmgr permite a los usuarios con credenciales administrativas crear y eliminar tarjetas inteligentes virtuales de TPM en un equipo. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>Parámetros de comando Create

El comando Create configura nuevas tarjetas inteligentes virtuales en el sistema del usuario. Devuelve el identificador de instancia de la tarjeta recién creada para consultarlos más adelante si se requiere la eliminación. La instancia de identificador tiene el formato **ROOT\SMARTCARDREADER\000n** donde **n** se inicia en 0 y se incrementa en 1 cada vez que cree una nueva tarjeta inteligente virtual.

|Parámetro|Descripción|
|---------|-----------|
|/name|Obligatorio. Indica el nombre de la nueva tarjeta inteligente virtual.|
|/AdminKey|Indica la clave de administrador deseado que se puede usar para restablecer el PIN de la tarjeta, si el usuario olvida el PIN.</br>**Valor predeterminado** especifica el valor predeterminado de 010203040506070801020304050607080102030405060708.</br>**Símbolo del sistema** pide al usuario que escriba un valor para la clave de administrador.</br>**ALEATORIO** da como resultado un valor aleatorio para la clave de administrador de una tarjeta que no se devuelve al usuario. Esto crea una tarjeta que podría no ser fáciles de administrar mediante el uso de las herramientas de administración de tarjeta inteligente. Cuando se genera con RANDOM, debe especificarse la clave de administrador como 48 caracteres hexadecimales.|
|/PIN|Indica el valor PIN de usuario deseado.</br>**Valor predeterminado** especifica el valor predeterminado PIN 12345678.</br>**Símbolo del sistema** pide al usuario que escriba un PIN en la línea de comandos. El PIN debe tener un mínimo de ocho caracteres, y puede contener números, caracteres y caracteres especiales.|
|/PUK|Indica el valor de clave de desbloqueo de PIN (PUK) deseado. El valor PUK debe ser un mínimo de ocho caracteres, y puede contener números, caracteres y caracteres especiales. Si se omite el parámetro, se crea sin un PUK la tarjeta.</br>**Valor predeterminado** especifica el valor predeterminado PUK 12345678.</br>**Símbolo del sistema** le pide al usuario que escriba un PUK en la línea de comandos.|
|/generate|Genera los archivos en el almacenamiento de la tarjeta inteligente virtual necesita para funcionar. Si el / generar parámetro se omite, es equivalente a la creación de una tarjeta sin este sistema de archivos. Una tarjeta sin un sistema de archivos se puede administrar sólo por un sistema de administración de tarjetas inteligentes, como Microsoft Configuration Manager.|
|/ Machine|Permite especificar el nombre de un equipo remoto en el que se puede crear la tarjeta inteligente virtual. Esto se puede usar en un entorno de dominio solo, y depende de DCOM. Para que el comando se ejecute correctamente en la creación de una tarjeta inteligente virtual en un equipo diferente, el usuario que ejecuta este comando debe ser miembro del grupo de administradores local en el equipo remoto.|
|/?|Muestra la ayuda para este comando.|

### <a name="parameters-for-destroy-command"></a>Parámetros de comando Destroy

El comando Destroy elimina de forma segura una tarjeta inteligente virtual desde el equipo del usuario.

> [!WARNING]
> Cuando se elimina una tarjeta inteligente virtual, no puede recuperarse.

|Parámetro|Descripción|
|---------|-----------|
|/ Instance|Especifica el identificador de instancia de la tarjeta inteligente virtual va a quitar. El identificador de instancia generó salida Tpmvscmgr.exe cuando se creó la tarjeta. El parámetro/Instance es un campo obligatorio para el comando Destroy.|
|/?|Muestra la ayuda para este comando.|

## <a name="remarks"></a>Comentarios

Pertenencia a la **administradores** grupo (o equivalente) en el equipo de destino es el requisito mínimo para ejecutar todos los parámetros de este comando.

Alfanuméricas entradas, se permite el conjunto de completa 127 caracteres ASCII.

## <a name="BKMK_Examples"></a>Ejemplos

El comando siguiente muestra cómo crear una tarjeta inteligente virtual que se pueden administrar más adelante mediante una herramienta de administración de tarjetas inteligentes iniciada desde otro equipo.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
Como alternativa, en lugar de usar una clave de administrador predeterminada, puede crear una clave de administrador en la línea de comandos. El comando siguiente muestra cómo crear una clave de administrador.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
El comando siguiente creará la tarjeta inteligente virtual no administrada que puede usarse para inscribir certificados.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
El comando siguiente crea una tarjeta inteligente virtual con una clave aleatoria de administrador. La clave se descarta automáticamente después de la cardis creado. Esto significa que si el usuario olvida el NIP o desea el cambio del PIN, el usuario debe eliminar la tarjeta y vuelva a crearlo. Para eliminar la tarjeta, el usuario puede ejecutar el comando siguiente.
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
donde \<Id. de instancia > es el valor que se imprime en la pantalla cuando el usuario creó la tarjeta. En concreto, para la primera tarjeta creada, el identificador de instancia es ROOT\SMARTCARDREADER\0000.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)