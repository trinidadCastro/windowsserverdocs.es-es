---
title: popd
description: Artículo de referencia para el comando pnputil, que cambia el directorio actual al directorio que se almacenó más recientemente con el comando Inserted.
ms.topic: reference
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 0d62716785546f9ca4259f99fbe9c5cd9ca9a718
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035233"
---
# <a name="popd"></a>popd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comando **popd** cambia el directorio actual al directorio que se almacenó más recientemente con el comando **Inserted** .

Cada vez que se usa el comando **Inserted** , se almacena un solo directorio para su uso. Sin embargo, puede almacenar varios directorios mediante el comando **Inserted** varias veces. Los directorios se almacenan secuencialmente en una pila virtual, por lo que si usa el comando **insertado** una vez, el directorio en el que se usa el comando se coloca en la parte inferior de la pila. Si vuelve a usar el comando, el segundo directorio se coloca en la parte superior de la primera. El proceso se repite cada vez que se usa el comando **Inserted** .

Si usa el comando **popd** , se quita el directorio de la parte superior de la pila y se cambia el directorio actual a ese directorio. Si vuelve a usar el comando **popd** , se quita el siguiente directorio de la pila. Si se habilitan las extensiones de comando, el comando **popd** quita todas las asignaciones de letra de unidad creadas por el comando **Inserted** .

## <a name="syntax"></a>Sintaxis

```
popd
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para cambiar el directorio actual de aquél en el que se ejecutó el programa de batch y, a continuación, volver a cambiarlo, escriba:

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

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [pushd](pushd.md)
