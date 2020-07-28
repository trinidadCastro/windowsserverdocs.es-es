---
title: Deshabilitar el almacenamiento en caché del lado cliente DNS en clientes DNS
description: En este artículo se describe cómo deshabilitar el almacenamiento en caché del lado cliente DNS en clientes DNS.
manager: dcscontentpm
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 74e7f3936418bd2f04234d07b2f600197e94b357
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182351"
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


Para deshabilitar la caché de DNS de forma permanente en Windows, use la herramienta controlador de servicio o la herramienta servicios para establecer el tipo de inicio del servicio cliente DNS en **deshabilitado**. Tenga en cuenta que el nombre del servicio cliente DNS de Windows también puede aparecer como "dnscache". 

> [!NOTE]
> Si se desactiva la memoria caché de la resolución DNS, el rendimiento general del equipo cliente disminuye y aumenta el tráfico de red de las consultas DNS. 

El servicio cliente DNS optimiza el rendimiento de la resolución de nombres DNS al almacenar los nombres resueltos previamente en la memoria. Si el servicio cliente DNS está desactivado, el equipo puede seguir resolviendo los nombres DNS mediante los servidores DNS de la red. 

Cuando la resolución de Windows recibe una respuesta, ya sea positiva o negativa, a una consulta, agrega esa respuesta a su caché y, por tanto, crea un registro de recursos DNS. El solucionador siempre comprueba la memoria caché antes de consultar un servidor DNS. Si un registro de recursos DNS está en la memoria caché, el solucionador utiliza el registro de la memoria caché en lugar de consultar un servidor. Este comportamiento acelera las consultas y reduce el tráfico de red de las consultas DNS. 

Puede usar la herramienta ipconfig para ver y vaciar la memoria caché de la resolución DNS. Para ver la memoria caché de la resolución DNS, ejecute el siguiente comando en un símbolo del sistema:

```cmd
ipconfig /displaydns 
```

Este comando muestra el contenido de la memoria caché de resolución DNS, incluidos los registros de recursos DNS que se cargan previamente desde el archivo de hosts y los nombres consultados recientemente que el sistema resolvió. Después de un tiempo, el solucionador descarta el registro de la memoria caché. El período de tiempo se especifica mediante el valor de período **de vida (TTL)** asociado al registro de recursos DNS. También puede vaciar la memoria caché manualmente. Después de vaciar la memoria caché, el equipo debe consultar de nuevo los servidores DNS para cualquier registro de recursos DNS que el equipo haya resuelto previamente. Para eliminar las entradas de la memoria caché de la resolución DNS, ejecute `ipconfig /flushdns` en un símbolo del sistema.

## <a name="using-the-registry-to-control-the-caching-time"></a>Usar el registro para controlar el tiempo de almacenamiento en caché

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/help/322756), por si se produjeran problemas.

El período de tiempo durante el que una respuesta positiva o negativa se almacena en caché depende de los valores de las entradas de la siguiente clave del registro:

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DNSCache\Parameters**

El TTL para las respuestas positivas es el menor de los siguientes valores: 

- El número de segundos especificado en la respuesta de la consulta que recibió la resolución

- El valor de la configuración del registro **MaxCacheTtl** .

>[!Note]
>- El TTL predeterminado para las respuestas positivas es de 86.400 segundos (1 día).
>- El TTL para las respuestas negativas es el número de segundos especificado en la configuración del registro MaxNegativeCacheTtl.
>- El TTL predeterminado para las respuestas negativas es de 5 segundos. antes de Windows 10, la versión 1703 el valor predeterminado era 900 segundos (15 minutos).
Si no desea almacenar en caché las respuestas negativas, establezca el valor del registro MaxNegativeCacheTtl en 0.

Para establecer la hora de almacenamiento en caché en un equipo cliente:

1. Inicie el editor del registro (Regedit.exe).

2. Busque y, a continuación, haga clic en la clave siguiente en el registro:

   **HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\Dnscache\Parameters**

3. En el menú Edición, seleccione nuevo, haga clic en valor DWORD y, a continuación, agregue los siguientes valores del registro:

   - Nombre del valor: MaxCacheTtl

     Tipo de datos: REG_DWORD

     Datos del valor: valor predeterminado 86400 segundos.

     Si reduce el valor de TTL máximo en la memoria caché de DNS del cliente a 1 segundo, esto da la impresión de que la memoria caché de DNS del cliente se ha deshabilitado.

   - Nombre del valor: MaxNegativeCacheTtl

     Tipo de datos: REG_DWORD

     Datos del valor: valor predeterminado de 5 segundos.

     Establezca el valor en 0 si no desea que se almacenen en caché las respuestas negativas.

4. Escriba el valor que desea utilizar y, a continuación, haga clic en Aceptar.

5. Salga del Editor del Registro.
