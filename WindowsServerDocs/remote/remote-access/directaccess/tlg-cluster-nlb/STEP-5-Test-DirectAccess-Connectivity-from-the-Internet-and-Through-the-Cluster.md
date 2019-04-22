---
title: PASO 5 probar la conectividad de DirectAccess desde Internet y a través del clúster
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
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49b3f6f68bf30ff197b51643f9f1b8f36cc76f19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825976"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>PASO 5 probar la conectividad de DirectAccess desde Internet y a través del clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Client1 está ahora preparado para realizar pruebas de DirectAccess.  
  
- Probar la conectividad de DirectAccess desde Internet. Conecte CLIENT1 a la red Internet simulada. Cuando se conecta a la red Internet simulada, el cliente se asigna direcciones IPv4 públicas. Cuando un cliente de DirectAccess se asigna una dirección IPv4 pública, intenta establecer una conexión con el servidor de acceso remoto mediante una tecnología de transición IPv6.  
  
- Probar la conectividad de cliente de DirectAccess a través del clúster. Pruebe la funcionalidad del clúster. Antes de comenzar las pruebas, se recomienda que apague EDGE1 y EDGE2 durante al menos cinco minutos. Hay varias razones para esto, lo que incluye los tiempos de espera de caché de ARP y cambios relacionados con NLB. Al validar la configuración de NLB en un laboratorio de pruebas, necesita su paciencia mientras los cambios de configuración no se reflejará inmediatamente en conectividad hasta que haya transcurrido un período de tiempo. Esto es importante tener en cuenta al realizar las siguientes tareas.  
  
    > [!TIP]  
    > Se recomienda borrar la caché de Internet Explorer antes de realizar este procedimiento y cada vez que se prueba la conexión a través de un servidor diferente del acceso remoto para asegurarse de que está probando la conexión y no recuperando las páginas Web de la memoria caché.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Probar la conectividad de DirectAccess desde Internet  
  
1.  Desconecta CLIENT1 del conmutador de red corporativa y conéctelo al conmutador de Internet. Espere 30 segundos.  
  
2.  En una ventana de Windows PowerShell con privilegios elevados, escriba **ipconfig /flushdns** y presione ENTRAR. Esto vacía entradas de resolución de nombres que todavía existan en la memoria caché del cliente DNS de cuando el equipo cliente se conectó a la red corporativa.  
  
3.  En la ventana de Windows PowerShell, escriba **Get-DnsClientNrptPolicy** y presione ENTRAR.  
  
    El resultado muestra la configuración actual de la tabla de directivas de resolución de nombres (NRPT). Estas configuraciones indican que todas las conexiones a. corp.contoso.com deben resolverse por el servidor DNS de acceso remoto, con el 2001:db8:1::2 de dirección IPv6. Asimismo, fíjate en la entrada NRPT que indica que existe una exención para el nombre nls.corp.contoso.com; los nombres de la lista de exenciones no reciben respuesta del servidor DNS de acceso remoto. Puede hacer ping a la dirección IP del servidor DNS de acceso remoto para confirmar la conectividad al servidor de acceso remoto; Por ejemplo, puede hacer ping 2001:db8:1::2.  
  
4.  En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Debería ver las respuestas de la dirección IPv6 de APP1, que en este caso es 2001:db8:1::3.  
  
5.  En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Deberías ver respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fdc9:9f4e:eb1b:7777::a00:4.  
  
    La capacidad de hacer ping APP2 es importante, porque el éxito indica que haya podido establecer una conexión con NAT64/DNS64, APP2 sea un recurso solo IPv4.  
  
6.  Deje la ventana de Windows PowerShell abierta para el siguiente procedimiento.  
  
7.  Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione ENTRAR. Verás el sitio web IIS predeterminado en APP1.  
  
8.  En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
9. En el **iniciar** , escriba**\\\App2\Files**, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto.  
  
    Esto demuestra que has sido capaz de conectarse a un servidor solo IPv4 utilizando SMB para obtener un recurso en el dominio de recursos.  
  
10. En el **iniciar** , escriba**wf.msc**, y, a continuación, presione ENTRAR.  
  
11. En el **Firewall de Windows con seguridad avanzada** de consola, tenga en cuenta que solo el **privada** o **perfil público** está activo. El Firewall de Windows debe habilitarse para que DirectAccess funcione correctamente. Si el Firewall de Windows está deshabilitado, no funciona la conectividad de DirectAccess.  
  
12. En el panel izquierdo de la consola, expanda el **supervisión** nodo y haga clic en el **reglas de seguridad de conexión** nodo. Debería ver las reglas de seguridad de la conexión activa: **DirectAccess Policy-ClientToCorp**, **ClientToDNS64NAT64PrefixExemption de directiva de DirectAccess**, **ClientToInfra de directiva de DirectAccess**, y **DirectAccess Policy-ClientToNlaExempt**. Desplácese hacia el panel central a la derecha para mostrar el **1 de los métodos de autenticación** y **2nd métodos de autenticación** columnas. Tenga en cuenta que la primera regla (ClientToCorp) utiliza Kerberos V5 para establecer el túnel de intranet y la tercera regla (ClientToInfra) utiliza NTLMv2 para establecer el túnel de infraestructura.  
  
13. En el panel izquierdo de la consola, expanda el **las asociaciones de seguridad** nodo y haga clic en el **modo principal** nodo. Tenga en cuenta las asociaciones de seguridad del túnel de infraestructura mediante NTLMv2 y la asociación de seguridad del túnel de intranet con Kerberos V5. Haga clic en la entrada que muestra **usuario (Kerberos V5)** como el **2 método de autenticación** y haga clic en **propiedades**. En el **General** pestaña, observe el **Id. Local de segunda autenticación** es **CORP\User1**, que indica que se puede autenticar correctamente al dominio CORP con User1 Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Probar la conectividad de cliente de DirectAccess a través del clúster  
  
1.  Realizar un apagado ordenado en EDGE2.  
  
    Puede usar el Administrador de equilibrio de carga de red para ver el estado de los servidores al ejecutar estas pruebas.  
  
2.  En CLIENTE1, en la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR. Esto vacía entradas de resolución de nombres que todavía existan en la caché DNS del cliente.  
  
3.  En la ventana de Windows PowerShell, haga ping a APP1 y APP2. Debería recibir respuestas de ambos de estos recursos.  
  
4.  En el **iniciar** , escriba**\\\app2\files**. Debería ver la carpeta compartida en el equipo de APP2. La capacidad de abrir el recurso compartido de archivos en APP2 indica que el segundo túnel, lo que requiere la autenticación Kerberos para el usuario, funciona correctamente.  
  
5.  Abra Internet Explorer y, a continuación, abra los sitios Web https://app1/ y https://app2/. La capacidad de abrir ambos sitios Web confirma que los túneles primeros y segundo están activos y en funcionamiento. Cierra Internet Explorer.  
  
6.  Inicie el equipo EDGE2.  
  
7.  En EDGE1 realizar un apagado correcto.  
  
8.  Espere cinco minutos y, a continuación, volver a CLIENT1. Realice los pasos 2 a 5. Esto confirma que CLIENT1 fue capaz de forma transparente conmutación por error a EDGE2 después EDGE1 dejó de estar disponible.
