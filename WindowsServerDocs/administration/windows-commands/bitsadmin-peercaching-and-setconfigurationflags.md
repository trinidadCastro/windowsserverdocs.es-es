---
title: bitsadmin-caché y setconfigurationflags
description: 'Temas de comandos de Windows para la **caché de bitsadmin y setconfigurationflags** : establece las marcas de configuración que determinan si el equipo puede servir contenido a los equipos del mismo nivel y puede descargar contenido desde los equipos del mismo nivel.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a65d54bcaa2bce26eb2b7c98250837ab09c7a423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381114"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin-caché y setconfigurationflags



Establece las marcas de configuración que determinan si el equipo puede servir contenido a los elementos del mismo nivel y puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Valor|El valor es un entero sin signo con la siguiente interpretación de los bits en la representación binaria:</br>-Permitir la descarga de los datos del trabajo desde un nodo del mismo nivel: Establecer el bit menos significativo</br>: Permite que los datos del trabajo se atiendan a los elementos del mismo nivel: Establezca el segundo bit de la derecha.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se especifican los datos del trabajo que se van a descargar de los elementos del mismo nivel para el trabajo denominado *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)