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
ms.openlocfilehash: b66b5edd2fa068923650f578d13f30631a8602ad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837478"
---
# <a name="popd"></a>popd

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el directorio actual al directorio que se almacenó más recientemente con el comando **Inserted** .
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
popd
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Cada vez que se usa el comando **Inserted** , se almacena un solo directorio para su uso. Sin embargo, puede almacenar varios directorios mediante el comando **Inserted** varias veces.
    Los directorios se almacenan secuencialmente en una pila virtual. Si usa el comando **insertado** una vez, el directorio en el que usa el comando se coloca en la parte inferior de la pila. Si vuelve a usar el comando, el segundo directorio se coloca en la parte superior de la primera. El proceso se repite cada vez que se usa el comando **Inserted** .
    Puede usar el comando **popd** para cambiar el directorio actual al directorio almacenado más recientemente por el comando **Inserted** . Si usa el comando **popd** , el directorio en la parte superior de la pila se quita de la pila y el directorio actual se cambia a ese directorio. Si vuelve a usar el comando **popd** , se quita el siguiente directorio de la pila.
-   Cuando se habilitan las extensiones de comando, el comando **popd** quita todas las asignaciones de letra de unidad creadas por la **extracción**.

## <a name="examples"></a><a name="BKMK_examples"></a>Example
En el ejemplo siguiente se muestra cómo se puede usar el comando **Inserted** y el comando **popd** en un programa por lotes para cambiar el directorio actual de aquél en el que se ejecutó el programa de batch y, a continuación, cambiarlo de nuevo:

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

