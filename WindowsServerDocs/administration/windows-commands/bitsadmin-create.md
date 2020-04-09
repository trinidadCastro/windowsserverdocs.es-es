---
title: bitsadmin create
description: Temas de comandos de Windows para **bitsadmin Create**, que crea un trabajo de transferencia con el nombre para mostrar especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a922d9f15aff0a9bd064a7e987920adf3a9107d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850818"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un trabajo de transferencia con el nombre para mostrar especificado. Los trabajos de descarga transfieren datos de un servidor a un archivo local. Los trabajos de carga transfieren datos de un archivo local a un servidor. Los trabajos de respuesta de carga transfieren datos de un archivo local a un servidor y reciben un archivo de respuesta del servidor.

Use el modificador de la [reanudación de bitsadmin](bitsadmin-resume.md) para activar el trabajo en la cola de transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| tipo | -  **/download** transfiere datos de un servidor a un archivo local.<p>-  **/upload** transfiere datos de un archivo local a un servidor.<p>-  **/upload-reply** transfiere datos de un archivo local a un servidor y recibe un archivo de respuesta del servidor.<p>El valor predeterminado de este parámetro es **/download** cuando no se especifica en la línea de comandos. Además, los tipos  **/upload** y **/upload-reply** no están disponibles en bits 1,2 y versiones anteriores. |
| displayname | Nombre para mostrar asignado al trabajo recién creado. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Crea un trabajo de descarga denominado *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
