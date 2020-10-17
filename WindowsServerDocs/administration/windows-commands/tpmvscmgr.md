---
title: tpmvscmgr
description: Artículo de referencia de tpmvscmgr, que es una herramienta de línea de comandos que permite a los usuarios con credenciales administrativas crear y eliminar tarjetas inteligentes virtuales de TPM en un equipo.
ms.topic: reference
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1dfa4a51c0ea75092e3abf885476e77d3c35435a
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156421"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

La herramienta de línea de comandos tpmvscmgr permite a los usuarios con credenciales administrativas crear y eliminar tarjetas inteligentes virtuales de TPM en un equipo.

## <a name="syntax"></a>Sintaxis

```
tpmvscmgr create [/name] [/adminkey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```

```
tpmvscmgr destroy [/instance <instanceID>] [/?]
```

### <a name="create-parameters"></a>Creación de parámetros

El comando **Create** configura nuevas tarjetas inteligentes virtuales en el sistema del usuario. También devuelve el identificador de instancia de la tarjeta recién creada para que se haga referencia a ella más adelante, si es necesario eliminarla. El identificador de instancia tiene el formato **ROOT\SMARTCARDREADER\000n** , donde **n** comienza en 0 y se incrementa en 1 cada vez que se crea una nueva tarjeta inteligente virtual.

| Parámetro | Descripción |
|--|--|
| /Name | Necesario. Indica el nombre de la nueva tarjeta inteligente virtual. |
| /adminkey | Indica la clave de administrador deseada que se puede usar para restablecer el PIN de la tarjeta si el usuario olvida el PIN. Esto puede incluir:<ul><li>**Default** : especifica el valor predeterminado de *010203040506070801020304050607080102030405060708*.</li><li>**Prompt** : pide al usuario que escriba un valor para la clave de administrador.</li><li>**Aleatorio** : genera un valor aleatorio para la clave de administrador de una tarjeta que no se devuelve al usuario. Esto crea una tarjeta que no se puede administrar mediante las herramientas de administración de tarjetas inteligentes. Al usar la opción RANDOM, la clave de administrador debe escribirse como caracteres hexadecimales 48.</li></ul> |
| /PIN | Indica el valor del PIN del usuario deseado.<ul><li>**Default** : especifica el PIN predeterminado de 12345678.</li><li>**Prompt** : pide al usuario que escriba un PIN en la línea de comandos. El PIN debe tener un mínimo de ocho caracteres y puede contener números, caracteres y caracteres especiales.</li></ul> |
| /PUK | Indica el valor de clave de desbloqueo de PIN deseado (PUK). El valor PUK debe tener un mínimo de ocho caracteres y puede contener números, caracteres y caracteres especiales. Si se omite el parámetro, la tarjeta se crea sin un PUK. Entre estas opciones se incluyen:<ul><li>**Default** : especifica el PUK predeterminado de *12345678*.</li><li>**Prompt** : solicita al usuario que escriba una PUK en la línea de comandos.</li></ul> |
| /generate | Genera los archivos en el almacenamiento que son necesarios para que funcione la tarjeta inteligente virtual. Si no usa el parámetro **/Generate** , es como si creara la tarjeta sin el sistema de archivos subyacente. Una tarjeta sin un sistema de archivos solo se puede administrar mediante un sistema de administración de tarjetas inteligentes, como Microsoft Configuration Manager. |
| /machine | Permite especificar el nombre de un equipo remoto en el que se puede crear la tarjeta inteligente virtual. Esto solo se puede usar en un entorno de dominio y se basa en DCOM. Para que el comando se ejecute correctamente en la creación de una tarjeta inteligente virtual en otro equipo, el usuario que ejecuta este comando debe ser miembro del grupo Administradores local en el equipo remoto. |
| /? | Muestra ayuda para este comando. |

### <a name="destroy-parameters"></a>Destruir parámetros

El comando **Destroy** elimina de forma segura una tarjeta inteligente virtual del equipo del usuario.

> [!WARNING]
> Si se elimina una tarjeta inteligente virtual, no se puede recuperar.

| Parámetro | Descripción |
|--|--|
| /instance | Especifica el identificador de instancia de la tarjeta inteligente virtual que se va a quitar. El instanceID se generó como resultado tpmvscmgr.exe cuando se creó la tarjeta. El parámetro **/Instance** es un campo obligatorio para el comando **Destroy** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- En el caso de las entradas alfanuméricas, se permite el conjunto ASCII de caracteres 127 completo.

## <a name="examples"></a>Ejemplos

Para crear una tarjeta inteligente virtual que se pueda administrar más adelante mediante una herramienta de administración de tarjetas inteligentes iniciada desde otro equipo, escriba:

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```

Como alternativa, en lugar de usar una clave de administrador predeterminada, puede crear una clave de administrador en la línea de comandos. El siguiente comando muestra cómo crear una clave de administrador.

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```

Para crear una tarjeta inteligente virtual no administrada que se pueda usar para inscribir certificados, escriba:

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```

Una tarjeta inteligente virtual se crea con una clave de administrador aleatoria. La clave se descarta automáticamente una vez creada la tarjeta. Esto significa que si el usuario olvida el PIN o desea cambiar el PIN, el usuario debe eliminar la tarjeta y volver a crearla.

Para eliminar la tarjeta, escriba:

```
tpmvscmgr.exe destroy /instance <instance ID>
```

Donde `<instanceID>` es el valor que se imprime en la pantalla cuando el usuario creó la tarjeta. En concreto, para la primera tarjeta creada, el ID. de instancia es *ROOT\SMARTCARDREADER\0000*.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
