---
title: Importar paquetes de datos en el servidor de caché hospedada (opcional)
description: Obtenga información acerca de cómo importar paquetes de datos y cargar previamente el contenido en los servidores de caché hospedada.
manager: brianlic
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 1c0626181ebdca7d6e78bb429a8d127089ad4446
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965120"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importar paquetes de datos en el servidor de caché hospedada \( opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para importar paquetes de datos y cargar previamente el contenido en los servidores de caché hospedada.

Este procedimiento es opcional porque no es necesario aplicar previamente hash y precargar contenido en los servidores de caché hospedada.

Si no \- carga previamente el contenido, los datos se agregan automáticamente a la caché hospedada a medida que los clientes lo descargan a través de la conexión WAN.

Para realizar este procedimiento debe ser miembro del grupo de administradores.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Para importar paquetes de datos en el servidor de caché hospedada

1. En el equipo servidor, abra Windows PowerShell con privilegios de administrador.

2. Escriba el comando siguiente, reemplazando el valor del parámetro – Path por la ubicación de la carpeta donde ha almacenado los paquetes de datos y, a continuación, presione Entrar.

    ```
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```

3. Si tiene más de un servidor de caché hospedada en el que desea cargar previamente el contenido, realice este procedimiento en cada servidor de caché hospedada.

Para continuar con esta guía, consulte [configuración del cliente detección automática de caché hospedada de cliente por punto de conexión de servicio](10-Bc-Client-By-Scp.md).
