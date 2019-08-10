---
title: Deshabilitar el almacenamiento en caché del lado cliente DNS en clientes DNS
description: En este artículo se describe cómo deshabilitar el almacenamiento en caché del lado cliente DNS en clientes DNS.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 0b20400029b462798587c2291431b5a7c3d61775
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917812"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>Deshabilitar el almacenamiento en caché del lado cliente DNS en clientes DNS

Windows contiene una caché DNS del lado cliente. La característica de almacenamiento en caché DNS del cliente puede generar una impresión falsa de que el equilibrio de carga "round robin" de DNS no se está produciendo desde el servidor DNS al equipo cliente de Windows. Cuando se usa el comando ping para buscar el mismo nombre de dominio de registro A, el cliente puede usar la misma dirección IP.  

## <a name="how-to-disable-client-side-caching"></a>Cómo deshabilitar el almacenamiento en caché del lado cliente

Para detener el almacenamiento en caché de DNS, ejecute cualquiera de los siguientes comandos:

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


Para deshabilitar la caché de DNS de forma permanente en Windows, use la herramienta controlador de servicio o la herramienta servicios para establecer el tipode inicio del servicio cliente DNS en deshabilitado. Tenga en cuenta que el nombre del servicio cliente DNS de Windows también puede aparecer como "dnscache". 

> [!NOTE]
> Si se desactiva la memoria caché de la resolución DNS, el rendimiento general del equipo cliente disminuye y aumenta el tráfico de red de las consultas DNS. 

El servicio cliente DNS optimiza el rendimiento de la resolución de nombres DNS al almacenar los nombres resueltos previamente en la memoria. Si el servicio cliente DNS está desactivado, el equipo puede seguir resolviendo los nombres DNS mediante los servidores DNS de la red. 

Cuando la resolución de Windows recibe una respuesta, ya sea positiva o negativa, a una consulta, agrega esa respuesta a su caché y, por tanto, crea un registro de recursos DNS. El solucionador siempre comprueba la memoria caché antes de consultar un servidor DNS. Si un registro de recursos DNS está en la memoria caché, el solucionador utiliza el registro de la memoria caché en lugar de consultar un servidor. Este comportamiento acelera las consultas y reduce el tráfico de red de las consultas DNS. 

Puede usar la herramienta ipconfig para ver y vaciar la memoria caché de la resolución DNS. Para ver la memoria caché de la resolución DNS, ejecute el siguiente comando en un símbolo del sistema:

```cmd
ipconfig /displaydns 
```

Este comando muestra el contenido de la memoria caché de resolución DNS, incluidos los registros de recursos DNS que se cargan previamente desde el archivo de hosts y los nombres consultados recientemente que el sistema resolvió. Después de un tiempo, el solucionador descarta el registro de la memoria caché. El período de tiempo se especifica mediante el valor de período **de vida (TTL)** asociado al registro de recursos DNS. También puede vaciar la memoria caché manualmente. Después de vaciar la memoria caché, el equipo debe consultar de nuevo los servidores DNS para cualquier registro de recursos DNS que el equipo haya resuelto previamente. Para eliminar las entradas de la memoria caché de la resolución DNS, `ipconfig /flushdns` ejecute en un símbolo del sistema.

## <a name="next-step"></a>Paso siguiente

Consulte deshabilitación del [almacenamiento en caché de DNS de cliente en Windows](https://support.microsoft.com/kb/318803) para obtener más información.
