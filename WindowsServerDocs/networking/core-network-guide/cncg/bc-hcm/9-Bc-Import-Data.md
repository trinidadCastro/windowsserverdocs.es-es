---
title: Importar paquetes de datos en el servidor de caché hospedada (opcional)
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 440ef1e04143cba09213ffea634aa9d4fea51dab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888006"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importar paquetes de datos en el servidor de caché hospedada \(opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para importar paquetes de datos y cargar previamente contenido en los servidores de caché hospedada.

Este procedimiento es opcional porque no es necesario para prehash y precarga de contenido en los servidores de caché hospedada.

Si lo hace no pre\-carga contenido, datos se agregan automáticamente a la caché hospedada como los clientes lo descargan a través de la conexión WAN.

Para realizar este procedimiento debe ser miembro del grupo de administradores.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Para importar paquetes de datos en el servidor de caché hospedada  

1. En el equipo del servidor, abra Windows PowerShell con privilegios de administrador.

2. Escriba el comando siguiente, reemplazando el valor para el parámetro – Path con la ubicación de la carpeta donde ha almacenado los paquetes de datos y, a continuación, presione ENTRAR.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Si tiene más de un servidor de caché hospedada en la que desea precargar contenido, debe realizar este procedimiento en cada servidor de caché hospedada.

Para continuar con esta guía, consulte [configurar cliente hospedados caché de la detección automática mediante el punto de conexión de servicio](10-Bc-Client-By-Scp.md).
