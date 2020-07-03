---
title: 'Actualización: ServerFiles'
description: Artículo de referencia para Update-ServerFiles, que actualiza los archivos de la carpeta compartida REMINST con los archivos más recientes que se almacenan en la carpeta%Windir%\System32\RemInst del servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79b9332d5962c7e3c50ea3d7c71f33111a0e74b8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930129"
---
# <a name="update-serverfiles"></a>Actualización: ServerFiles

Actualiza los archivos de la carpeta compartida REMINST usando los archivos más recientes que se almacenan en la carpeta%Windir%\System32\RemInst del servidor. Para garantizar la validez de la instalación de servicios de implementación de Windows, debe ejecutar este comando una vez después de cada actualización de servidor, Service Pack instalación o actualización de los archivos de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/Server:\<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|

## <a name="examples"></a>Ejemplos

Para actualizar los archivos, escriba uno de los siguientes:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)