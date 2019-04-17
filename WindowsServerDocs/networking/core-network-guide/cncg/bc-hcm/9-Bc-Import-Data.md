---
title: Importar paquetes de datos en el servidor de memoria caché hospedada (opcional)
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0bd8f12ab76c8e2bf03ba79ce46a4cbea2f4dc5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importar paquetes de datos en hospedado servidor \(Optional\) en caché

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar este procedimiento para importar paquetes de datos y precargar contenido en los servidores de la memoria caché hospedada.

Este procedimiento es opcional, ya que no es necesarios para el contenido prehash y precarga en los servidores de la memoria caché hospedada.

Si lo haces, no pre\ cargar contenido, se agregan automáticamente a la memoria caché hospedada datos como los clientes descargan con la conexión WAN.

Debe ser miembro del grupo Administradores para realizar este procedimiento.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Para importar los paquetes de datos en el servidor de la memoria caché hospedada  

1. En el equipo servidor, abre Windows PowerShell con privilegios de administrador.

2. Escribe el comando siguiente, sustituyendo el valor para – parámetro de ruta de acceso con la ubicación de carpeta donde se almacenan los paquetes de datos y, a continuación, presione ENTRAR.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Si tienes más de un servidor de memoria caché hospedada donde quieras precargar contenido, realizar este procedimiento en cada servidor de la memoria caché hospedada.

Para continuar con esta guía, consulte [configurar cliente hospedadas caché la detección automática al punto de conexión de servicio](10-Bc-Client-By-Scp.md).
