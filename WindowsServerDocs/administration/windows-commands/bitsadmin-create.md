---
title: bitsadmin create
description: 'Temas de comandos de Windows para **bitsadmin Create** : crea un trabajo de transferencia con el nombre para mostrar especificado.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9f6d641d44c56ea4ff11f48a725367de7dcf472a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381811"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un trabajo de transferencia con el nombre para mostrar especificado. Los trabajos de descarga transfieren datos de un servidor a un archivo local. Los trabajos de carga transfieren datos de un archivo local a un servidor. Los trabajos de respuesta de carga transfieren datos de un archivo local a un servidor y reciben un archivo de respuesta del servidor.

Use el modificador de la [reanudación de bitsadmin](bitsadmin-resume.md) para activar el trabajo en la cola de transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|type|-    **/download** transfiere datos de un servidor a un archivo local.<br />-    **/upload** transfiere datos de un archivo local a un servidor.<br />-    **/upload-reply** transfiere datos de un archivo local a un servidor y recibe un archivo de respuesta del servidor.<br />: El valor predeterminado de este parámetro es **/download** cuando no se especifica en la línea de comandos.|
|DisplayName|Nombre para mostrar asignado al trabajo recién creado.|

**BITS 1,2 y versiones anteriores**: Los tipos/upload y/Upload-Reply no están disponibles.

## <a name="BKMK_examples"></a>Example

Crea un trabajo de descarga denominado *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
