---
title: Eliminación de SC. exe
description: Obtenga información acerca de cómo anular el registro de servicios con la utilidad SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 284012cf6799df52832e62c3eea1b2f0fcd84805
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850116"
---
# <a name="scexe-delete"></a>Eliminación de SC. exe

Elimina una subclave de servicio del registro. Si el servicio se está ejecutando o si otro proceso tiene un identificador abierto para el servicio, el servicio se marca para su eliminación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NombreDeServidor>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por \\ \\ejemplo, mi Server). Para ejecutar SC. exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

No se recomienda usar SC. exe para eliminar servicios de sistema operativo integrados, como DHCP, DNS o Internet Information Services. Para instalar, quitar o volver a configurar roles de sistema operativo, servicios y componentes, consulte [instalación o desinstalación de roles, servicios de rol o características](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md) .

## <a name="examples"></a>Ejemplos

Para eliminar la subclave de servicio **NewServ** del registro en el equipo local, escriba:
```
sc.exe delete newserv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
