---
title: bitsadmin getnotifyflags
description: Tema de comandos de Windows para **bitsadmin getnotifyflags**, que recupera las marcas de notificación para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3138baea05f793cfb587d3f8fb669d446daea6b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850588"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

Recupera las marcas de notificación para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="remarks"></a>Comentarios

El trabajo puede contener una o varias de las siguientes marcas de notificación:

| Bandera | Descripción |
| ----- | ----- |
| 0x001 | Genera un evento cuando se han transferido todos los archivos del trabajo. |
| 0x002 | Genera un evento cuando se produce un error. |
| 0x004 | Deshabilite las notificaciones. |
| 0x008 | Genera un evento cuando se modifica el trabajo o se realiza el progreso de la transferencia. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recuperan las marcas de notificación del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)