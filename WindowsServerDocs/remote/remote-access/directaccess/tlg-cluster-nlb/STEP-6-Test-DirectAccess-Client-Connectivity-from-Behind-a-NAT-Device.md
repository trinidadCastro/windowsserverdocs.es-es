---
title: Paso 6 probar la Conectividad del cliente de DirectAccess desde detrás de un dispositivo NAT
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 083905f03a9a8f021d223f2ab0e7f0a5141f0e42
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946551"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>Paso 6 probar la Conectividad del cliente de DirectAccess desde detrás de un dispositivo NAT

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando un cliente de DirectAccess está conectado a Internet desde detrás de un dispositivo NAT o un servidor proxy web, el cliente de DirectAccess usa Teredo o IP-HTTPS para conectarse al servidor de acceso remoto.

Si el dispositivo NAT habilita el puerto UDP 3544 de salida para la dirección IP pública del servidor de acceso remoto, se utiliza Teredo. Si no es posible acceder a Teredo, el cliente de DirectAccess vuelve a IP-HTTPS a través del puerto TCP 443 de salida, lo que permite el acceso a través de firewalls o servidores proxy web a través del puerto SSL tradicional.

Si el servidor proxy web requiere autenticación, se producirá un error en la conexión IP-HTTPS. También se producirá un error en las conexiones IP-HTTPS si el servidor proxy web realiza una inspección SSL saliente, debido a que la sesión HTTPS se finaliza en el servidor proxy web en lugar de en el servidor de acceso remoto. En esta sección se realizarán las mismas pruebas que se llevaron a cabo en la sección anterior al conectarse a través de una conexión 6to4.

Los siguientes procedimientos deben efectuarse en ambos equipos cliente:

1. Probar la conectividad de Teredo. El primer conjunto de pruebas se realiza cuando el cliente de DirectAccess está configurado para usar Teredo. Esta es la configuración automática cuando el dispositivo NAT permite el acceso saliente al puerto UDP 3544.

2. Pruebe la conectividad IP-HTTPS. El segundo conjunto de pruebas se realiza cuando el cliente de DirectAccess está configurado para usar IP-HTTPS. Para demostrar la conectividad IP-HTTPS, Teredo debe deshabilitarse en los equipos cliente.

> [!TIP]
> Se recomienda que borre la memoria caché de Internet Explorer antes de llevar a cabo estos procedimientos para asegurarse de que está probando la conexión y no recupera las páginas del sitio web de la memoria caché.

## <a name="prerequisites"></a>Requisitos previos

Antes de efectuar estas pruebas, desconecta CLIENT1 del conmutador de Internet y conéctalo al conmutador Homenet. Si se te pregunta qué tipo de red deseas para definir la red actual, selecciona **Red doméstica**.

Inicia EDGE1 y EDGE2 si todavía no se están ejecutando.

## <a name="test-teredo-connectivity"></a>Probar la conectividad Teredo

1. En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados, escriba **ipconfig/all** y presione Entrar.

2. Examina el resultado del comando ipconfig.

   CLIENT1 está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Cuando el cliente de DirectAccess está detrás de un dispositivo NAT y se le asigna una dirección IPv4 privada, la tecnología de transición IPv6 preferida es Teredo. Si observas el resultado del comando ipconfig, deberías ver una sección para la pseudointerfaz de túnel Teredo para el adaptador de túnel seguida de la descripción Adaptador de túnel Teredo de Microsoft, con una dirección IP que empieza por 2001, lo que es coherente con una dirección Teredo. Si no ve la sección Teredo, habilite Teredo con el siguiente comando: **netsh interface Teredo set state enterpriseclient** y vuelva a ejecutar el comando ipconfig. No verás ninguna puerta de enlace predeterminada para el adaptador de túnel Teredo.

3. En la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar.

   De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a Internet.

4. En la ventana de Windows PowerShell, escriba **Get-DnsClientNrptPolicy** y presione Entrar.

   El resultado muestra la configuración actual de la tabla de directivas de resolución de nombres (NRPT). Dicha configuración indica que todas las conexiones a .corp.contoso.com deben resolverse a través del servidor DNS de acceso remoto, con la siguiente dirección IPv6: 2001:db8:1::2. Asimismo, fíjate en la entrada NRPT que indica que existe una exención para el nombre nls.corp.contoso.com; los nombres de la lista de exenciones no reciben respuesta del servidor DNS de acceso remoto. Puedes hacer ping a la dirección IP del servidor DNS de acceso remoto para confirmar la conectividad al servidor de acceso remoto; por ejemplo, puedes hacer ping a 2001:db8:1::2 en este ejemplo.

5. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.

6. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Deberías ver respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fdc9:9f4e:eb1b:7777::a00:4.

7. Deje abierta la ventana de Windows PowerShell para el procedimiento siguiente.

8. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione Entrar. Verás el sitio web IIS predeterminado en APP1.

9. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verás el sitio web predeterminado en APP2.

10. En la pantalla **Inicio** , escriba <strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.

## <a name="test-ip-https-connectivity"></a>Probar la conectividad IP-HTTPS

1. Abra una ventana de Windows PowerShell con privilegios elevados, escriba **netsh interface Teredo Set state disabled** y presione Entrar. Esto deshabilita Teredo en el equipo cliente y permite que el equipo cliente se configure a sí mismo para usar IP-HTTPS. Cuando se completa el comando, aparece la respuesta **Aceptar**.

2. En la ventana de Windows PowerShell, escriba **ipconfig/all** y presione Entrar.

3. Examina el resultado del comando ipconfig. El equipo está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Teredo está deshabilitado y el cliente de DirectAccess vuelve a IP-HTTPS. Si observas el resultado del comando ipconfig, verás una sección para el adaptador de túnel iphttpsinterface con una dirección IP que empieza por 2001:db8:1:100, lo que es coherente con una dirección IP-HTTPS basada en el prefijo que se estableció al configurar DirectAccess. No verás ninguna puerta de enlace predeterminada para el adaptador de túnel IP-HTTPS.

4. En la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar. De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a corpnet.

5. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.

6. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Deberías ver respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fdc9:9f4e:eb1b:7777::a00:4.

7. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione Entrar. Verás el sitio IIS predeterminado en APP1.

8. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verás el sitio web predeterminado en APP2.

9. En la pantalla **Inicio** , escriba <strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.
