---
title: PASO 13 probar la conectividad de DirectAccess desde detrás de un dispositivo NAT
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a9324e7c5b72f60422b1263e76c7d5e14cccf4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880976"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>PASO 13 probar la conectividad de DirectAccess desde detrás de un dispositivo NAT

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando un cliente de DirectAccess está conectado a Internet desde detrás de un dispositivo NAT o un servidor proxy web, el cliente de DirectAccess usa Teredo o IP-HTTPS para conectarse al servidor de acceso remoto. Si el dispositivo NAT habilita el puerto UDP 3544 a la dirección IP pública del servidor de acceso remoto, se usa Teredo. Si no es posible acceder a Teredo, el cliente de DirectAccess vuelve a IP-HTTPS a través del puerto TCP 443 de salida, lo que permite el acceso a través de firewalls o servidores proxy web a través del puerto SSL tradicional. Si el servidor proxy web requiere autenticación, se producirá un error en la conexión IP-HTTPS. También se producirá un error en las conexiones IP-HTTPS si el servidor proxy web realiza una inspección SSL saliente, debido a que la sesión HTTPS se finaliza en el servidor proxy web en lugar de en el servidor de acceso remoto.  
  
Los siguientes procedimientos deben efectuarse en ambos equipos cliente:  
  
1. Probar la conectividad Teredo. El primer conjunto de pruebas se realizan cuando el cliente de DirectAccess está configurado para usar Teredo. Esta es la configuración automática cuando el dispositivo NAT permite el acceso saliente al puerto UDP 3544. En primer lugar, ejecutar las pruebas en CLIENT1 y luego ejecutar las pruebas en CLIENT2.  
  
2. Probar la conectividad IP-HTTPS. El segundo conjunto de pruebas se realizan cuando el cliente de DirectAccess está configurado para usar IP-HTTPS. Para demostrar la conectividad IP-HTTPS, Teredo debe deshabilitarse en los equipos cliente. En primer lugar, ejecutar las pruebas en CLIENT1 y luego ejecutar las pruebas en CLIENT2.  
  
## <a name="prerequisites"></a>Requisitos previos  
Inicia EDGE1 y 2 EDGE1 si ya no se está ejecutando y asegúrese de que están conectados a la subred de Internet.  
  
Antes de realizar estas pruebas, desconecta CLIENT1 y CLIENT2 del conmutador de Internet y conéctelos al conmutador Homenet. Si te pregunta qué tipo de red deseas definir la red actual, seleccione **red doméstica**.  
  
## <a name="TeredoCLIENT1"></a>Probar la conectividad Teredo  
  
1.  En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados.  
  
2.  Habilitar el adaptador Teredo, tipo **teredo de interfaz de netsh establece estado enterpriseclient**, y, a continuación, presione ENTRAR.  
  
3.  En la ventana de Windows PowerShell, escriba **ipconfig/all** y presione ENTRAR.  
  
4.  Examina el resultado del comando ipconfig.  
  
    El equipo está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Cuando el cliente de DirectAccess está detrás de un dispositivo NAT y se le asigna una dirección IPv4 privada, la tecnología de transición IPv6 preferida es Teredo. Si observa la salida del comando ipconfig, debería ver una sección para el adaptador de túnel Teredo pseudo-interfaz de túnel y, a continuación, una descripción del adaptador de túnel Teredo de Microsoft, con una dirección IP que empieza por 2001: 0 coherente con un Teredo dirección. Debería ver la puerta de enlace predeterminada para el adaptador de túnel Teredo como '::'.  
  
5.  En la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR.  
  
    De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a Internet.  
  
6.  En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.  
  
7.  En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Debería ver las respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fd**c9:9f4e:eb1b**: 7777::a00:4. Tenga en cuenta que los valores en negrita variará en cómo se genera la dirección.  
  
8.  En la ventana de Windows PowerShell, escriba **ping app1 de 2** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 de 2-APP1, 2001:db8:2::3.  
  
9. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione ENTRAR. Verá el sitio Web de IIS predeterminado en APP1 de 2.  
  
10. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
11. En el **iniciar** , escriba**\\\App2\Files**, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.  
  
12. Repita este procedimiento en CLIENT2.  
  
## <a name="IPHTTPS_CLIENT1"></a>Probar la conectividad IP-HTTPS  
  
1.  En CLIENT1, abra un elevado ventana de Windows PowerShell y escriba **teredo de interfaz de netsh establece el estado deshabilitado** y presione ENTRAR. Esto deshabilita Teredo en el equipo cliente y permite que el equipo cliente se configure a sí mismo para usar IP-HTTPS. Cuando se completa el comando, aparece la respuesta **Aceptar** .  
  
2.  En la ventana de Windows PowerShell, escriba **ipconfig/all** y presione ENTRAR.  
  
3.  Examina el resultado del comando ipconfig. El equipo está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Teredo está deshabilitado y el cliente de DirectAccess vuelve a IP-HTTPS. Al examinar la salida del comando ipconfig, verás una sección para el adaptador de túnel iphttpsinterface con una dirección IP que comienza con 2001:db8:1:1000 o 2001:db8:2:2000 coherente con una dirección IP-HTTPS en función de los prefijos que estaban estableció al configurar DirectAccess. No verá ninguna puerta de enlace predeterminada para el adaptador de túnel IPHTTPSInterface.  
  
4.  En la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR. De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a corpnet.  
  
5.  En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.  
  
6.  En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Debería ver las respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fd**c9:9f4e:eb1b**: 7777::a00:4. Tenga en cuenta que los valores en negrita variará en cómo se genera la dirección.  
  
7.  En la ventana de Windows PowerShell, escriba **ping app1 de 2** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 de 2-APP1, 2001:db8:2::3.  
  
8.  Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione ENTRAR. Verá el sitio Web de IIS predeterminado en APP1 de 2.  
  
9. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
10. En el **iniciar** , escriba**\\\App2\Files**, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.  
  
11. Repita este procedimiento en CLIENT2.  
  


