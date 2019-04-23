---
title: agregar
description: 'Tema de los comandos de Windows para **add_1** : volúmenes se agrega al conjunto de volúmenes de instantáneas, o agrega alias para el entorno de alias.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549c8560774f004a60926ce568c850fd1b71c7f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889956"
---
# <a name="add"></a>agregar


los volúmenes se agrega al conjunto de volúmenes de instantáneas, o agrega alias para el entorno de alias. Si se usa sin subcomandos, **agregar** se enumeran los volúmenes actuales y los alias.

> [!NOTE]
> Los alias no se agregan al entorno de alias hasta que se crea la instantánea. Se deben agregar los alias que necesita inmediatamente mediante el uso de **agregar alias**.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>Agregar subcomandos

|Subcomando|Descripción|
|----------|-----------|
|volumen|Agrega un volumen al conjunto de copia de instantáneas, que es el conjunto de volúmenes de instantáneas. Consulte [agregar volumen](add-volume.md) para la sintaxis y los parámetros.|
|alias|Agrega el nombre y valor dados en el entorno de alias. Consulte [agregar alias](add-alias.md) para la sintaxis y los parámetros.|
|/?|Muestra la Ayuda en la línea de comandos.|

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar los volúmenes que se agregan y los alias que están actualmente en el entorno, escriba:
```
add
```
La siguiente salida muestra que se ha agregado la unidad C para el conjunto de copia sombra:
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)