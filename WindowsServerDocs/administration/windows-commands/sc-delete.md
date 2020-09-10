---
title: Sc.exe eliminar
description: Obtenga información acerca de cómo anular el registro de servicios con la utilidad sc.exe
ms.topic: reference
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8ce9eb203fd9db68629ce5412836eb694eb67162
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637105"
---
# <a name="scexe-delete"></a>Sc.exe eliminar

Elimina una subclave de servicio del registro. Si el servicio se está ejecutando o si otro proceso tiene un identificador abierto para el servicio, el servicio se marca para su eliminación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, mi \\ \\ Server). Para ejecutar SC.exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

No se recomienda usar sc.exe para eliminar servicios de sistema operativo integrados, como DHCP, DNS o Internet Information Services. Para instalar, quitar o volver a configurar roles de sistema operativo, servicios y componentes, consulte [instalación o desinstalación de roles, servicios de rol o características](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md) .

## <a name="examples"></a>Ejemplos

Para eliminar la subclave de servicio **NewServ** del registro en el equipo local, escriba:
```
sc.exe delete newserv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
