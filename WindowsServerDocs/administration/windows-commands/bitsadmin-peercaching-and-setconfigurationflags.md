---
title: setconfigurationflags y bitsadmin caché del mismo nivel
description: Tema de los comandos de Windows para **bitsadmin caché del mismo nivel y setconfigurationflags** -establece las marcas de configuración que determinan si el equipo puede servir contenido a elementos del mismo nivel y puede descargar contenido de elementos del mismo nivel.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 22408d4aab7f5ea374511bc16751d911a84644f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813336"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>setconfigurationflags y bitsadmin caché del mismo nivel



Establece las marcas de configuración que determinan si el equipo puede servir contenido a elementos del mismo nivel y puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Valor|El valor es un entero sin signo con la interpretación de la representación binaria los bits siguiente:</br>-Permitir que los datos del trabajo se descargan desde un punto: Establecer el bit menos significativo</br>-Permitir que los datos del trabajo proporcionarse a elementos del mismo nivel: Establezca el bit 2 de la derecha.|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente especifica los datos del trabajo descarga del mismo nivel para el trabajo denominado *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)