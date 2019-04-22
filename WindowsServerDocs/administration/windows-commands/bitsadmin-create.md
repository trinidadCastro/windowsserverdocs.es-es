---
title: bitsadmin create
description: Tema de los comandos de Windows para **crear bitsadmin** -crea un trabajo de transferencia con el nombre para mostrar especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6ce5a4fdc21d879bf0a265e3c4185d83311464a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817196"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un trabajo de transferencia con el nombre para mostrar especificado. Descargar trabajos de transferencia de datos de un servidor en un archivo local. Cargar datos de transferencia de trabajos desde un archivo local a un servidor. Los trabajos de respuesta de carga transferir datos desde un archivo local a un servidor y recepción un archivo de respuesta del servidor.

Use la [bitsadmin reanudar](bitsadmin-resume.md) conmutador para activar el trabajo en la cola de transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|type|-   **/ Descargar** transfiere datos desde un servidor a un archivo local.<br />-   **/ Cargar** transfiere datos desde un archivo local a un servidor.<br />-   **/ Carga-respuesta** transfiere datos desde un archivo local a un servidor y recibe un archivo de respuesta del servidor.<br />: Valor predeterminado este parámetro es **/descargar** cuando no se especifica en la línea de comandos.|
|DisplayName|El nombre para mostrar asignado al trabajo recién creado.|

**BITS 1.2 y versiones anteriores**: Los tipos de /Upload-Reply y/Upload no están disponibles.

## <a name="BKMK_examples"></a>Ejemplos

Crea un trabajo de descarga denominado *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
