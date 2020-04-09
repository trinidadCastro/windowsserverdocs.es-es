---
title: bitsadmin-caché y getconfigurationflags
description: Tema de comandos de Windows para la caché de sistema de **bitsadmin** y **getconfigurationflags**, que obtiene las marcas de configuración que determinan si el equipo atiende el contenido a los elementos del mismo nivel y si puede descargar contenido de los equipos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be8d6a719d63c8e9c6250320560b6ce21274c680
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850178"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin-caché y getconfigurationflags

Obtiene las marcas de configuración que determinan si el equipo atiende el contenido a los elementos del mismo nivel y si puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se obtienen las marcas de configuración para el trabajo denominado *myDownloadJob*.

```
C:\> bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)