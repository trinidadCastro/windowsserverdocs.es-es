---
title: PASO 6 probar la conectividad de cliente de DirectAccess desde detrás de un dispositivo NAT
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a48bb9b012a81e91fbbe0ad914637dacac5a8a5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883006"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>PASO 6 probar la conectividad de cliente de DirectAccess desde detrás de un dispositivo NAT

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando un cliente de DirectAccess está conectado a Internet desde detrás de un dispositivo NAT o un servidor proxy web, el cliente de DirectAccess usa Teredo o IP-HTTPS para conectarse al servidor de acceso remoto. 

Si el dispositivo NAT habilita el puerto UDP 3544 a la dirección IP pública del servidor de acceso remoto, se usa Teredo. Si no es posible acceder a Teredo, el cliente de DirectAccess vuelve a IP-HTTPS a través del puerto TCP 443 de salida, lo que permite el acceso a través de firewalls o servidores proxy web a través del puerto SSL tradicional. 

Si el servidor proxy web requiere autenticación, se producirá un error en la conexión IP-HTTPS. También se producirá un error en las conexiones IP-HTTPS si el servidor proxy web realiza una inspección SSL saliente, debido a que la sesión HTTPS se finaliza en el servidor proxy web en lugar de en el servidor de acceso remoto. En esta sección se realizarán las mismas pruebas que se llevaron a cabo en la sección anterior al conectarse a través de una conexión 6to4.  
  
Los siguientes procedimientos deben efectuarse en ambos equipos cliente:  
  
1. Probar la conectividad Teredo. El primer conjunto de pruebas se realizan cuando el cliente de DirectAccess está configurado para usar Teredo. Esta es la configuración automática cuando el dispositivo NAT permite el acceso saliente al puerto UDP 3544.  
  
2. Probar la conectividad IP-HTTPS. El segundo conjunto de pruebas se realizan cuando el cliente de DirectAccess está configurado para usar IP-HTTPS. Para demostrar la conectividad IP-HTTPS, Teredo debe deshabilitarse en los equipos cliente.  
  
> [!TIP]  
> Se recomienda que borre la caché de Internet Explorer antes de realizar estos procedimientos para asegurarse de que se está probando la conexión y no recuperando páginas Web de la memoria caché.  
  
## <a name="prerequisites"></a>Requisitos previos

Antes de efectuar estas pruebas, desconecta CLIENT1 del conmutador de Internet y conéctalo al conmutador Homenet. Si se te pregunta qué tipo de red deseas para definir la red actual, selecciona **Red doméstica**.  
  
Inicia EDGE1 y EDGE2 si todavía no se están ejecutando.  
  
## <a name="test-teredo-connectivity"></a>Probar la conectividad Teredo  
  
1.  En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados, escriba **ipconfig/all** y presione ENTRAR.  
  
2.  Examina el resultado del comando ipconfig.  
  
    CLIENT1 está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Cuando el cliente de DirectAccess está detrás de un dispositivo NAT y se le asigna una dirección IPv4 privada, la tecnología de transición IPv6 preferida es Teredo. Si observa la salida del comando ipconfig, debería ver una sección para el adaptador de túnel Teredo pseudo-interfaz de túnel y, a continuación, una descripción del adaptador de túnel Teredo de Microsoft, con una dirección IP que empieza por 2001: coherente con un Teredo dirección. Si no ve la sección Teredo, habilite Teredo con el siguiente comando: **netsh interface Teredo set state enterpriseclient** y vuelva a ejecutar el comando ipconfig. No verás ninguna puerta de enlace predeterminada para el adaptador de túnel Teredo.  
  
3.  En la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR.  
  
    De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a Internet.  
  
4.  En la ventana de Windows PowerShell, escriba **Get-DnsClientNrptPolicy** y presione ENTRAR.  
  
    El resultado muestra la configuración actual de la tabla de directivas de resolución de nombres (NRPT). Dicha configuración indica que todas las conexiones a .corp.contoso.com deben resolverse a través del servidor DNS de acceso remoto, con la siguiente dirección IPv6: 2001:db8:1::2. Asimismo, fíjate en la entrada NRPT que indica que existe una exención para el nombre nls.corp.contoso.com; los nombres de la lista de exenciones no reciben respuesta del servidor DNS de acceso remoto. Puedes hacer ping a la dirección IP del servidor DNS de acceso remoto para confirmar la conectividad al servidor de acceso remoto; por ejemplo, puedes hacer ping a 2001:db8:1::2 en este ejemplo.  
  
5.  En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.  
  
6.  En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Deberías ver respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fdc9:9f4e:eb1b:7777::a00:4.  
  
7.  Deje la ventana de Windows PowerShell abierta para el siguiente procedimiento.  
  
8.  Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione ENTRAR. Verás el sitio web IIS predeterminado en APP1.  
  
9. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
10. En el **iniciar** , escriba**\\\App2\Files**, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.  
  
## <a name="test-ip-https-connectivity"></a>Probar la conectividad IP-HTTPS  
  
1.  Abra una ventana de Windows PowerShell con privilegios elevados, escriba **teredo de interfaz de netsh establece el estado deshabilitado** y presione ENTRAR. Esto deshabilita Teredo en el equipo cliente y permite que el equipo cliente se configure a sí mismo para usar IP-HTTPS. Cuando se completa el comando, aparece la respuesta **Aceptar** .  
  
2.  En la ventana de Windows PowerShell, escriba **ipconfig/all** y presione ENTRAR.  
  
3.  Examina el resultado del comando ipconfig. El equipo está ahora conectado a Internet desde detrás de un dispositivo NAT y se le ha asignado una dirección IPv4 privada. Teredo está deshabilitado y el cliente de DirectAccess vuelve a IP-HTTPS. Al examinar la salida del comando ipconfig, verás una sección para el adaptador de túnel iphttpsinterface con una dirección IP que empieza por 2001:db8:1:100 coherente con una dirección IP-HTTPS basada en el prefijo que se estableció al configurar DirectAccess. No verás ninguna puerta de enlace predeterminada para el adaptador de túnel IP-HTTPS.  
  
4.  En la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR. De este modo, se vaciarán las entradas de resolución de nombres que todavía existan en la caché de DNS de cliente desde el momento en que el equipo cliente se conectó a corpnet.  
  
5.  En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 de APP1, 2001:db8:1::3.  
  
6.  En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Deberías ver respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fdc9:9f4e:eb1b:7777::a00:4.  
  
7.  Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione ENTRAR. Verás el sitio IIS predeterminado en APP1.  
  
8.  En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
9. En el **iniciar** , escriba**\\\App2\Files**, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto. Esto demuestra que has sido capaz de conectarte a un servidor solo IPv4 utilizando SMB para obtener un recurso en un host solo IPv4.
