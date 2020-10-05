---
title: 'Actualización: ServerFiles'
description: Artículo de referencia para Update-ServerFiles, que actualiza los archivos de la carpeta compartida REMINST con los archivos más recientes que se almacenan en la carpeta%Windir%\System32\RemInst del servidor.
ms.topic: reference
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1a984c789a309395dcee0d93a6ba0877c44aab22
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731372"
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