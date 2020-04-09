---
title: tpmvscmgr
description: Windows Commands topic for tpmvscmgr, que es una herramienta de línea de comandos que permite a los usuarios con credenciales administrativas crear y eliminar tarjetas inteligentes virtuales de TPM en un equipo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4411e0ec3c75cd768b2fe32ad26b17331328e3ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832738"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

La herramienta de línea de comandos Tpmvscmgr permite a los usuarios con credenciales administrativas crear y eliminar tarjetas inteligentes virtuales de TPM en un equipo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

#### <a name="parameters-for-create-command"></a>Parámetros para Create (comando)

El comando CREATE configura nuevas tarjetas inteligentes virtuales en el sistema del usuario. Devuelve el identificador de instancia de la tarjeta recién creada para que se haga referencia a ella más adelante si se requiere la eliminación. El identificador de instancia tiene el formato **ROOT\SMARTCARDREADER\000n** , donde **n** comienza en 0 y se incrementa en 1 cada vez que se crea una nueva tarjeta inteligente virtual.

|Parámetro|Descripción|
|---------|-----------|
|/Name|Obligatorio. Indica el nombre de la nueva tarjeta inteligente virtual.|
|/AdminKey|Indica la clave de administrador deseada que se puede usar para restablecer el PIN de la tarjeta si el usuario olvida el PIN.</br>**Valor predeterminado** Especifica el valor predeterminado de 010203040506070801020304050607080102030405060708.</br>**Preguntar** Solicita al usuario que escriba un valor para la clave de administrador.</br>**Aleatoriedad** Da como resultado una configuración aleatoria para la clave de administrador de una tarjeta que no se devuelve al usuario. Esto crea una tarjeta que no se puede administrar mediante las herramientas de administración de tarjetas inteligentes. Cuando se genera con RANDOM, la clave de administrador debe escribirse como caracteres hexadecimales 48.|
|/PIN|Indica el valor del PIN del usuario deseado.</br>**Valor predeterminado** Especifica el PIN predeterminado de 12345678.</br>**Preguntar** Solicita al usuario que escriba un PIN en la línea de comandos. El PIN debe tener un mínimo de ocho caracteres y puede contener números, caracteres y caracteres especiales.|
|/PUK|Indica el valor de clave de desbloqueo de PIN deseado (PUK). El valor PUK debe tener un mínimo de ocho caracteres y puede contener números, caracteres y caracteres especiales. Si se omite el parámetro, la tarjeta se crea sin un PUK.</br>**Valor predeterminado** Especifica el PUK predeterminado de 12345678.</br>**Preguntar** Solicita al usuario que escriba una PUK en la línea de comandos.|
|/generate|Genera los archivos en el almacenamiento que son necesarios para que funcione la tarjeta inteligente virtual. Si se omite el parámetro/Generate, es equivalente a crear una tarjeta sin este sistema de archivos. Una tarjeta sin un sistema de archivos solo se puede administrar mediante un sistema de administración de tarjetas inteligentes, como Microsoft Configuration Manager.|
|/Machine|Permite especificar el nombre de un equipo remoto en el que se puede crear la tarjeta inteligente virtual. Esto solo se puede usar en un entorno de dominio y se basa en DCOM. Para que el comando se ejecute correctamente en la creación de una tarjeta inteligente virtual en otro equipo, el usuario que ejecuta este comando debe ser miembro del grupo Administradores local en el equipo remoto.|
|/?|Muestra ayuda para este comando.|

#### <a name="parameters-for-destroy-command"></a>Parámetros del comando Destroy

El comando Destroy elimina de forma segura una tarjeta inteligente virtual del equipo del usuario.

> [!WARNING]
> Cuando se elimina una tarjeta inteligente virtual, no se puede recuperar.

|Parámetro|Descripción|
|---------|-----------|
|/instance|Especifica el identificador de instancia de la tarjeta inteligente virtual que se va a quitar. El instanceID se generó como resultado de Tpmvscmgr. exe cuando se creó la tarjeta. El parámetro/Instance es un campo obligatorio para el comando Destroy.|
|/?|Muestra ayuda para este comando.|

## <a name="remarks"></a>Comentarios

La pertenencia al grupo **administradores** (o equivalente) en el equipo de destino es el requisito mínimo para ejecutar todos los parámetros de este comando.

En el caso de las entradas alfanuméricas, se permite el conjunto ASCII de caracteres 127 completo.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

El siguiente comando muestra cómo crear una tarjeta inteligente virtual que se puede administrar más adelante mediante una herramienta de administración de tarjetas inteligentes iniciada desde otro equipo.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```
Como alternativa, en lugar de usar una clave de administrador predeterminada, puede crear una clave de administrador en la línea de comandos. El siguiente comando muestra cómo crear una clave de administrador.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```
El siguiente comando creará la tarjeta inteligente virtual no administrada que se puede usar para inscribir certificados.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```
El siguiente comando creará una tarjeta inteligente virtual con una clave de administrador aleatoria. La clave se descarta automáticamente una vez creada la tarjeta. Esto significa que si el usuario olvida el PIN o desea cambiar el PIN, el usuario debe eliminar la tarjeta y volver a crearla. Para eliminar la tarjeta, el usuario puede ejecutar el siguiente comando.
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
donde \<ID. de instancia > es el valor que se imprime en la pantalla cuando el usuario creó la tarjeta. En concreto, para la primera tarjeta creada, el ID. de instancia es ROOT\SMARTCARDREADER\0000.

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)