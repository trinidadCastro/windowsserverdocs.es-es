---
title: El comando UPDATE-ServerFiles
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93eeb0deaa527921db35f4ab955d2ccc46b57d7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385847"
---
# <a name="the-update-serverfiles-command"></a>El comando UPDATE-ServerFiles



Actualiza los archivos de la carpeta compartida REMINST usando los archivos más recientes que se almacenan en la carpeta%Windir%\System32\RemInst del servidor. Para garantizar la validez de la instalación de servicios de implementación de Windows, debe ejecutar este comando una vez después de cada actualización de servidor, Service Pack instalación o actualización de los archivos de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/Server: \<Server nombre >]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|

## <a name="BKMK_examples"></a>Example

Para actualizar los archivos, escriba uno de los siguientes:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)