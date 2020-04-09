---
title: append
description: Temas de comandos de Windows para **anexar**, que permite a los programas abrir archivos de datos en los directorios especificados, como si estuvieran en el directorio actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95bbc607ef297e7cf67da2e388884882356ef744
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851328"
---
# <a name="append"></a>append

Permite a los programas abrir archivos de datos en directorios especificados como si estuvieran en el directorio actual. Si se usa sin parámetros, **Append** muestra la lista de directorios anexados.

> [!NOTE]
> Este comando no se admite en Windows 10.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[\<Drive>:]<Path>` | Especifica una unidad y un directorio para anexar. |
| `/x:on` | Aplica los directorios anexados a las búsquedas de archivos y las aplicaciones de inicio. |
| `/x:off` | Solo se aplica a las solicitudes para abrir archivos. La opción **/x: OFF** es la configuración predeterminada. |
| `/path:on` | Aplica los directorios anexados a las solicitudes de archivos que ya especifican una ruta de acceso. **/path: on** es el valor predeterminado. |
| `/path:off` | Desactiva el efecto de **/path: on**. |
| `/e` | Almacena una copia de la lista de directorios anexada en una variable de entorno denominada APPEND. **/e** solo se puede usar la primera vez que se usa **Append** después de iniciar el sistema. |
| `;` | Borra la lista de directorios anexados. |
| `/?` | Muestra la Ayuda en el símbolo del sistema. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para borrar la lista de directorios anexados, escriba:

```
append ;
```

Para almacenar una copia del directorio anexado en una variable de entorno denominada APPEND, escriba:

```
append /e
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
