---
title: Problemas para eliminar un nodo
description: En este artículo se describen los problemas que se producen al quitar nodos de la pertenencia al clúster de conmutación por error activa.
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: e69b110db8f631b74c89e046f724367b4d60dbad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970042"
---
# <a name="having-a-problem-with-nodes-being-removed-from-active-failover-cluster-membership"></a>Problema con los nodos que se quitan de la pertenencia al clúster de conmutación por error activa

En este artículo se describe cómo resolver los problemas en los que los nodos se quitan de la pertenencia al clúster de conmutación por error activa aleatoriamente.

## <a name="symptoms"></a>Síntomas

Cuando se produce el problema, verá eventos como los que se registraron en el registro de eventos del sistema:

![Evento 1135](media/problem-nodes-failover-cluster/1135-1.png)

Este evento se registrará en todos los nodos del clúster, excepto en el nodo que se ha quitado. La razón de este evento es que uno de los nodos del clúster marcó ese nodo como inactivo. A continuación, notifica a todos los demás nodos del evento. Cuando se notifica a los nodos, estos no continúan y anulan las conexiones de latido al nodo inactivo.

## <a name="what-caused-the-node-to-be-marked-down"></a>Qué hizo que el nodo se marcara hacia abajo

Todos los nodos de un clúster de conmutación por error de Windows 2008 o 2008 R2 se comunican entre sí a través de las redes que están configuradas para permitir la comunicación de red de clústeres en esta red. Los nodos enviarán paquetes de latido entre estas redes a todos los demás nodos. Los demás nodos deben recibir estos paquetes y, a continuación, se devuelve una respuesta. Cada nodo del clúster tiene sus propios latidos que se van a supervisar para asegurarse de que la red está activa y los demás nodos están en funcionamiento. El ejemplo siguiente debería ayudarle a aclarar esto:

![Nodos](media/problem-nodes-failover-cluster/Node2.png)

Si no se devuelve alguno de estos paquetes, se considera que el latido específico no se ha realizado correctamente. Por ejemplo, W2K8-R2-NODO2 envía una solicitud y recibe una respuesta de W2K8-R2-NODO1 a un paquete de latidos, por lo que determina la red y el nodo está activo.  Si W2K8-R2-NODO1 envía una solicitud a W2K8-R2-NODO2 y W2K8-R2-NODO1 no obtiene la respuesta, se considera un latido perdido y W2K8-R2-NODO1 realiza un seguimiento de la misma.  Esta respuesta perdida puede tener W2K8-R2-NODO1 Mostrar la red como inactiva hasta que se reciba otra solicitud de latido.

De forma predeterminada, los nodos de clúster tienen un límite de cinco errores en 5 segundos antes de que la conexión se marque como inactiva. Por tanto, si W2K8-R2-NODO1 no recibe la respuesta cinco veces en el período de tiempo, tiene en cuenta que la ruta determinada a W2K8-R2-NODO2 es inactiva. Si se siguen considerando otras rutas, W2K8-R2-NODO2 permanecerá como miembro activo.

Si todas las rutas están marcadas para W2K8-R2-NODO2, se quitan de la pertenencia al clúster de conmutación por error activa y se registra el evento 1135 que se ve en la primera sección. En W2K8-R2-NODO2, el servicio de clúster se termina y, a continuación, se reinicia para que pueda intentar volver a unirse al clúster.

Para obtener más información sobre cómo se administran rutas específicas que se desconectan con tres o más nodos, consulte el blog [sobre redes de clústeres con particiones](/archive/blogs/askcore/partitioned-cluster-networks) escrito por Jeff Hughes.

## <a name="now-that-we-know-how-the-heartbeat-process-works-what-are-some-of-the-known-causes-for-the-process-to-fail"></a>Ahora que sabemos cómo funciona el proceso de latido, ¿cuáles son algunas de las causas conocidas del proceso de error?

1. Errores reales de hardware de red. Si el paquete se pierde en la conexión en algún lugar de los nodos, se producirá un error en los latidos. Un seguimiento de red de ambos nodos implicados lo revelará.

2. Es posible que el perfil de las conexiones de red esté rebotando del dominio al público y de nuevo al dominio. Durante la transición de estos cambios, se puede bloquear la e/s de red. Puede comprobar si este es el caso examinando el registro operativo del perfil de red. Puede encontrar este registro abriendo el Visor de eventos y navegando a: aplicaciones y servicios Logs\Microsoft\Windows\NetworkProfile\Operational. Examine los eventos de este registro en el nodo mencionado en el ID. de evento: 1135 y compruebe si el perfil ha cambiado en este momento. Si es así, consulte el artículo de KB [el perfil de ubicación de red cambia de "dominio" a "público" en Windows 7 o en Windows Server 2008 R2](https://support.microsoft.com/help/2524478/the-network-location-profile-changes-from-domain-to-public-in-windows).

3. Tiene IPv6 habilitado en los servidores, pero tiene las dos reglas siguientes deshabilitadas para entrante y saliente en el Firewall de Windows:

    - Redes principales: anuncio de detección de vecinos
    - Redes principales: solicitud de detección de vecinos

4. El software antivirus podría interferir también con este proceso. Si sospecha que esto es así, deshabilite o desinstale el software. Hágalo bajo su responsabilidad, ya que no estará protegido contra virus en este momento.

5. La latencia de la red también podría hacer que esto suceda. Es posible que los paquetes no se pierdan entre los nodos, pero es posible que no lleguen a los nodos lo suficientemente rápido antes de que expire el período de tiempo de espera.

6. IPv6 es el protocolo predeterminado que usará el clúster de conmutación por error para sus latidos. El propio latido es un paquete de red de unidifusión UDP que se comunica a través del puerto 3343. Si hay conmutadores, firewalls o enrutadores que no están configurados correctamente para permitir el tráfico a través de, puede experimentar problemas como este.

7. Las actualizaciones de la Directiva de seguridad IPsec también pueden producir este problema. El problema específico es que durante una actualización de la Directiva de grupo de IPSec todas las asociaciones de seguridad (SAs) de IPsec se anulan por el Firewall de Windows con seguridad avanzada (WFAS). Mientras esto sucede, se bloquea toda la conectividad de red. Al volver a negociar las asociaciones de seguridad si hay retrasos en la realización de la autenticación con Active Directory, estos retrasos (donde se bloquea toda la comunicación de red) también impedirán que los latidos del clúster pasen y provocarán que la supervisión del estado del clúster detecte los nodos como inactivos si no responden dentro del umbral de 5 segundos.

8. Controladores o firmware de tarjetas de red antiguos o no actualizados.  En ocasiones, un problema de configuración simple de la tarjeta de red o del conmutador también puede provocar la pérdida de latidos.

9. Es posible que se produzcan pérdidas de paquetes en tarjetas de red modernas y tarjetas de red virtual.  Para realizar el seguimiento, abra el monitor de rendimiento y agregue el contador "red Interface\Packets recibida descartada".  Este contador es acumulativo y solo aumenta hasta que se reinicia el servidor.  Ver un gran número de paquetes descartados puede ser un signo de que los búferes de recepción de la tarjeta de red están configurados demasiado bajos o que el servidor funciona lentamente y no puede controlar el tráfico entrante.  Cada fabricante de tarjetas de red elige si desea exponer esta configuración en las propiedades de la tarjeta de red, por lo que debe consultar el sitio web del fabricante para obtener información sobre cómo aumentar estos valores y se deben usar los valores recomendados.  Si está ejecutando en VMware, en el siguiente blog se habla de esto con un poco más de detalle, incluido cómo saber si este es el problema, así como apuntar al artículo de VMWare sobre la configuración que se va a cambiar.

    [Nodos que se quitan de la pertenencia al clúster de conmutación por error en VMWare ESX](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)

Estos son los motivos más comunes por los que se registran estos eventos, pero también podría haber otras razones. El punto de este blog era ofrecer información sobre el proceso y ofrecer también ideas de lo que se debe buscar. Algunos generarán los valores siguientes en sus valores máximos para intentar detener este problema.

|Parámetro|Valor predeterminado|Intervalo|
|---|---|---|
|**SameSubnetDelay**|1000 milisegundos|250-2000 milisegundos|
|**CrossSubnetDelay**|1000 milisegundos|250-4000 milisegundos|
|**SameSubnetThreshold**|5|3-10|
|**CrossSubnetThreshold**|5|3-10|
||||

Aumentar estos valores hasta su máximo puede hacer que la eliminación del evento y el nodo desaparezca, simplemente enmascara el problema. No corrige nada. Lo mejor es encontrar la causa principal de los errores de latido y solucionarlo. La única necesidad real de aumentar estos valores es en un escenario de varios sitios en el que los nodos residen en ubicaciones diferentes y la latencia de red no se puede solucionar.
