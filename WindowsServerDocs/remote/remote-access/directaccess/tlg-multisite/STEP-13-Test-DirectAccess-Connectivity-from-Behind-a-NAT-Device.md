---
title: Paso 13 probar la conectividad de DirectAccess desde detrás de un dispositivo NAT
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3ad38999df70c7ed8e6088687723090911dccbff
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950041"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>Paso 13 probar la conectividad de DirectAccess desde detrás de un dispositivo NAT

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando un cliente de DirectAccess está conectado a Internet desde detrás de un dispositivo NAT o un servidor proxy web, el cliente de DirectAccess usa Teredo o IP-HTTPS para conectarse al servidor de acceso remoto. Si el dispositivo NAT habilita el puerto UDP 3544 de salida para la dirección IP pública del servidor de acceso remoto, se utiliza Teredo. Si no es posible acceder a Teredo, el cliente de DirectAccess vuelve a IP-HTTPS a través del puerto TCP 443 de salida, lo que permite el acceso a través de firewalls o servidores proxy web a través del puerto SSL tradicional. Si el servidor proxy web requiere autenticación, se producirá un error en la conexión IP-HTTPS. También se producirá un error en las conexiones IP-HTTPS si el servidor proxy web realiza una inspección SSL saliente, debido a que la sesión HTTPS se finaliza en el servidor proxy web en lugar de en el servidor de acceso remoto.

Los siguientes procedimientos deben efectuarse en ambos equipos cliente:

1. Probar la conectividad de Teredo. El primer conjunto de pruebas se realiza cuando el cliente de DirectAccess está configurado para usar Teredo. Esta es la configuración automática cuando el dispositivo NAT permite el acceso saliente al puerto UDP 3544. En primer lugar, ejecute las pruebas en CLIENT1 y, a continuación, ejecute las pruebas en cliente2.

2. Pruebe la conectividad IP-HTTPS. El segundo conjunto de pruebas se realiza cuando el cliente de DirectAccess está configurado para usar IP-HTTPS. Para demostrar la conectividad IP-HTTPS, Teredo debe deshabilitarse en los equipos cliente. En primer lugar, ejecute las pruebas en CLIENT1 y, a continuación, ejecute las pruebas en cliente2.

## <a name="prerequisites"></a>Requisitos previos
Inicie EDGE1 y 2-EDGE1 si aún no se están ejecutando y asegúrese de que están conectados a la subred de Internet.

Antes de realizar estas pruebas, desconecte CLIENT1 y cliente2 del conmutador de Internet y conéctelas al conmutador HomeNet Si se le pregunta qué tipo de red desea definir la red actual, seleccione **red doméstica**.

## <a name="test-teredo-connectivity"></a><a name="TeredoCLIENT1"></a>Probar la conectividad Teredo

1. En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados.

2. Habilite el adaptador de Teredo, escriba **netsh interface Teredo Set State enterpriseclient** y, a continuación, presione Entrar.

3. En la ventana de Windows PowerShell, escriba **ipconfig/all** y presione Entrar.

4. Examina el resultado del comando ipconfig.

   El equipo está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Cuando el cliente de DirectAccess está detrás de un dispositivo NAT y se le asigna una dirección IPv4 privada, la tecnología de transición IPv6 preferida es Teredo. Si observa la salida del comando ipconfig, debería ver una sección para la tunelización Teredo del adaptador de túnel Pseudo-Interface y, a continuación, una descripción del adaptador de túnel Teredo de Microsoft, con una dirección IP que empieza por 2001:0 coherente y que es una dirección Teredo. Debería ver la puerta de enlace predeterminada indicada para el adaptador de túnel Teredo como "::".

5. En la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar.

   De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a Internet.

6. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.

7. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Debería ver las respuestas de la dirección NAT64 asignada por EDGE1 a APP2, que en este caso es FD **C9:9f4e: eb1b**: South:: A00:4. Tenga en cuenta que los valores en negrita variarán debido a cómo se genera la dirección.

8. En la ventana de Windows PowerShell, escriba **ping 2-app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 de 2 a APP1, 2001: db8:2:: 3.

9. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione Entrar. Verá el sitio web de IIS predeterminado en 2-APP1.

10. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verás el sitio web predeterminado en APP2.

11. En la pantalla **Inicio** , escriba <strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.

12. Repita este procedimiento en cliente2.

## <a name="test-ip-https-connectivity"></a><a name="IPHTTPS_CLIENT1"></a>Probar la conectividad IP-HTTPS

1. En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados y escriba **netsh interface Teredo Set state disabled** y presione Entrar. Esto deshabilita Teredo en el equipo cliente y permite que el equipo cliente se configure a sí mismo para usar IP-HTTPS. Cuando se completa el comando, aparece la respuesta **Aceptar**.

2. En la ventana de Windows PowerShell, escriba **ipconfig/all** y presione Entrar.

3. Examina el resultado del comando ipconfig. El equipo está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Teredo está deshabilitado y el cliente de DirectAccess vuelve a IP-HTTPS. Cuando examine la salida del comando ipconfig, verá una sección para el adaptador de túnel iphttpsinterface con una dirección IP que empieza por 2001: db8:1: 1000 o 2001: db8:2: 2000 coherente con esta es una dirección IP-HTTPS basada en los prefijos que se configuraron al configurar DirectAccess. No verá una puerta de enlace predeterminada para el adaptador de túnel IPHTTPSInterface.

4. En la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar. De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a corpnet.

5. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.

6. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Debería ver las respuestas de la dirección NAT64 asignada por EDGE1 a APP2, que en este caso es FD **C9:9f4e: eb1b**: South:: A00:4. Tenga en cuenta que los valores en negrita variarán debido a cómo se genera la dirección.

7. En la ventana de Windows PowerShell, escriba **ping 2-app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 de 2 a APP1, 2001: db8:2:: 3.

8. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione Entrar. Verá el sitio web de IIS predeterminado en 2-APP1.

9. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verás el sitio web predeterminado en APP2.

10. En la pantalla **Inicio** , escriba <strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.

11. Repita este procedimiento en cliente2.



