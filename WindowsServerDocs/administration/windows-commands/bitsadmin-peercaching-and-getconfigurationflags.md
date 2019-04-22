---
title: getconfigurationflags y bitsadmin caché del mismo nivel
description: 'Tema de los comandos de Windows para **bitsadmin caché del mismo nivel y getconfigurationflags** : Obtiene las marcas de configuración que determinan si el equipo sirve contenido a elementos del mismo nivel y puede descargar contenido de elementos del mismo nivel.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6afa39993cf90b2d71b6b681680c3b4e1fd9b56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826356"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>getconfigurationflags y bitsadmin caché del mismo nivel



Obtiene las marcas de configuración que determinan si el equipo sirve contenido a elementos del mismo nivel y puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente obtiene las marcas de la configuración del trabajo denominado *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)