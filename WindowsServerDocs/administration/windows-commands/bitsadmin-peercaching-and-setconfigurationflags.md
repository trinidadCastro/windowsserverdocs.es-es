---
title: bitsadmin-caché y setconfigurationflags
description: Tema de comandos de Windows para la caché de sistema de **bitsadmin** y **setconfigurationflags**, que establece las marcas de configuración que determinan si el equipo puede servir contenido a los equipos del mismo nivel y si puede descargar contenido de elementos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebaa09da2d4594d2762e67dc5884dd15cf4d1da8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850138"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin-caché y setconfigurationflags

Establece las marcas de configuración que determinan si el equipo puede servir contenido a los elementos del mismo nivel y si puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| value | Entero sin signo con la siguiente interpretación de los bits en la representación binaria:<ul><li> Para permitir que los datos del trabajo se descarguen de un elemento del mismo nivel, establezca el bit menos significativo.</li><li>Para permitir que los datos del trabajo se atiendan a los elementos del mismo nivel, establezca el segundo bit de la derecha.</li></ul>|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se especifican los datos del trabajo que se van a descargar de los elementos del mismo nivel para el trabajo denominado *myDownloadJob*.

```
C:\> bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)