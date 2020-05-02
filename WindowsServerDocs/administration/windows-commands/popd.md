---
title: popd
description: Obtenga información acerca de cómo cambiar el directorio al directorio que se almacenó más recientemente con el comando Inserted.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a1638f8d0d62b730578cb21fac5a74e58ad06b74
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723295"
---
# <a name="popd"></a>popd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el directorio actual al directorio que se almacenó más recientemente con el comando **Inserted** .


## <a name="syntax"></a>Sintaxis
```
popd
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   Cada vez que se usa el comando **Inserted** , se almacena un solo directorio para su uso. Sin embargo, puede almacenar varios directorios mediante el comando **Inserted** varias veces.
    Los directorios se almacenan secuencialmente en una pila virtual. Si usa el comando **insertado** una vez, el directorio en el que usa el comando se coloca en la parte inferior de la pila. Si vuelve a usar el comando, el segundo directorio se coloca en la parte superior de la primera. El proceso se repite cada vez que se usa el comando **Inserted** .
    Puede usar el comando **popd** para cambiar el directorio actual al directorio almacenado más recientemente por el comando **Inserted** . Si usa el comando **popd** , el directorio en la parte superior de la pila se quita de la pila y el directorio actual se cambia a ese directorio. Si vuelve a usar el comando **popd** , se quita el siguiente directorio de la pila.
-   Cuando se habilitan las extensiones de comando, el comando **popd** quita todas las asignaciones de letra de unidad creadas por la **extracción**.

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos
Para ver cómo puede usar el comando de **Inserted** y el comando **popd** en un programa de batch para cambiar el directorio actual de aquél en el que se ejecutó el programa de batch y, a continuación, cámbielo de nuevo:

```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>Referencias adicionales
-   [pushd](pushd.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

