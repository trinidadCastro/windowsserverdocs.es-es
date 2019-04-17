---
title: Detalles técnicos de virtualización de red de Hyper-V en Windows Server 2016
description: En este tema proporciona información técnica sobre la virtualización de la red de Hyper-V en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: af2b2a0b151601124bb473c465e7d5a97f2b9150
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Detalles técnicos de virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server 2016

La virtualización de servidor permite varias instancias de servidor ejecutar simultáneamente en un único host físico pero las instancias de servidor se aíslan entre sí. Cada máquina virtual básicamente funciona como si es el único servidor que ejecuta en el equipo físico.  
  
Virtualización de la red proporciona una funcionalidad similar, en qué varias redes virtuales (posiblemente con direcciones IP superpuestas) se ejecutan en la misma infraestructura de red física y cada red virtual funciona como si se trata de la red virtual solo ejecuta en la infraestructura de red compartida. Figura 1 muestra esta relación.  
  
![Virtualización de servidor frente a la virtualización de la red](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
Figura 1: Virtualización frente a la virtualización de la red  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Conceptos de virtualización de Hyper-V red  
Virtualización de red en Hyper-V (HNV), un cliente o inquilino se define como "propietario" de un conjunto de subredes IP que se implementan en una empresa o centro de datos. Un cliente puede ser una organización o empresa con varios departamentos o unidades de negocio en un centro de datos privado que requieren el aislamiento de red o un inquilino en el centro de datos públicos que está hospedado por un proveedor de servicios. Cada cliente puede tener uno o más [redes virtuales](#VirtualNetworks) en el centro de datos y cada elemento virtual red consta de una o más [Virtual subredes](#VirtualSubnets).  
  
Hay dos implementaciones HNV que estarán disponibles en Windows Server 2016: HNVv1 y HNVv2.  
  
-   **HNVv1**  
  
    HNVv1 es compatible con Windows Server 2012 R2 y System Center 2012 R2 Virtual Machine Manager (VMM). Configuración de HNVv1 se basa en la administración de WMI y los cmdlets de Windows PowerShell (que se facilita mediante System Center VMM) para definir la configuración de aislamiento y dirección de cliente (CA) - red virtual - asignaciones de dirección física (PA) y el enrutamiento. Ninguna de las características adicional se han agregado a HNVv1 en Windows Server 2016 y no están pensados nuevas características.  
    
    • ESTABLECE la agrupación y HNV V1 no son compatibles con la plataforma.
    
    O usar los usuarios de puertas de enlace HA NVGRE al usar LBFO equipo o ningún equipo. O
    
    O controlador de red de uso implementado puertas de enlace con conjunto habían asociado conmutador.

  
-   **HNVv2**  
  
    Una cantidad considerable de nuevas características se incluye en HNVv2 que se implementa mediante Azure Virtual filtrado de plataforma (VFP) reenvío de extensión en el conmutador de Hyper-V. HNVv2 está totalmente integrado con Microsoft Azure pila que incluye el nuevo controlador de red en la pila de Software definido de redes (SDN).  Se define la directiva de red virtual a través de Microsoft [controlador de red](../../../sdn/technologies/network-controller/Network-Controller.md) usando un RESTful NorthBound (NB) API y conectadas a un agente de Host a través de varios SouthBound interfaces (SBI) incluidos OVSDB. La directiva de programas de agente de Host en la extensión VFP del conmutador de Hyper-V que se está aplicando.  
  
    > [!IMPORTANT]  
    > En este tema se centra en HNVv2.  
  
### <a name="VirtualNetworks"></a>Red virtual  
  
-   Cada red virtual consta de una o varias subredes virtuales. Una red virtual constituye un límite de aislamiento en las máquinas virtuales dentro de una red virtual sólo se pueden comunicar entre sí. Tradicionalmente, este aislamiento se aplicara con VLAN con un intervalo de direcciones IP segregado y 802.1q etiqueta o un identificador de VLAN Pero con HNV, se aplica el aislamiento mediante NVGRE o VXLAN encapsulación para crear redes de superposición con la posibilidad de subredes IP entre los clientes o inquilinos de superposición.  
  
-   Cada red virtual tiene un identificador de dominio de enrutamiento único (RDID) en el equipo host. Este RDID aproximadamente se asigna a un identificador de recurso para identificar la red virtual recursos REST en el controlador de red. Se hace referencia a la recursos REST de red virtual con un espacio de nombres de identificador uniforme de recursos (URI) con el identificador de recurso anexado.  
  
### <a name="VirtualSubnets"></a>Subredes virtuales  
  
-   Una subred virtual implementa la semántica de subred de IP de capa 3 para las máquinas virtuales en la misma subred virtual. La subred virtual constituye un dominio de difusión (similar a una VLAN) y se aplica el aislamiento, utilizando el Id. de red de inquilino NVGRE (TNI) o el identificador de red VXLAN (VNI).  
  
-   Cada subred virtual pertenece a una sola red virtual (RDID), y se le asigna un único Virtual subred ID (VSID) mediante la TNI VNI clave o en el encabezado de paquete encapsulado. El VSID debe ser único en el centro de datos y está en el intervalo 4096 a 2 ^ 2 de 24.  
  
Una ventaja de la red virtual y el dominio de enrutamiento clave es que permite a los clientes mostrar sus propios topologías de red (por ejemplo, subredes IP) en la nube. Figura 2 muestra un ejemplo donde la Corp Contoso tiene dos redes independientes, la investigación y D Net y la red de ventas. Dado que estas redes tienen diferentes dominios enrutamiento identificadores, no pueden interactuar con ellos. Es decir, Contoso R y D Net está aislado de Internet de las ventas de Contoso aunque ambos son propiedad de Contoso Corporation. Contoso R y D Net contiene tres subredes virtuales. Ten en cuenta que el RDID y VSID sean únicas dentro de un centro de datos.  
  
![Redes de los clientes y subredes virtuales](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
Figura 2: Redes de los clientes y subredes virtuales  
  
**Reenvío de nivel 2**  
  
En la figura 2, las máquinas virtuales de VSID 5001 puede tener sus paquetes reenviados en las máquinas virtuales que también están en 5001 VSID mediante el conmutador de Hyper-V. Los paquetes de entrada de una máquina virtual en VSID 5001 se envían a un VPort en el conmutador de Hyper-V específicas. Se aplican las reglas de entrada (por ejemplo, encapsulación) y asignaciones (por ejemplo, un encabezado encapsulación) mediante el conmutador de Hyper-V para estos paquetes. Los paquetes se reenvían a un VPort diferentes en el conmutador de Hyper-V (si la máquina virtual de destino se adjunta al mismo host) o a un conmutador de Hyper-V diferente en un host diferente (si la máquina virtual de destino se encuentra en un host diferente).  
  
**Capa enrutamiento 3**  
  
Del mismo modo, las máquinas virtuales de VSID 5001 puede tener sus paquetes que se enrutan a máquinas virtuales en VSID 5002 o VSID 5003 Router distribuido HNV que está presente en VSwitch de cada host Hyper-V. Al proporcionar el paquete a la Hyper-V cambiar, HNV actualiza la VSID del paquete entrante para el VSID de la máquina virtual de destino. Esto ocurrirá solo si ambos VSIDs están en el mismo RDID.  Por lo tanto, los adaptadores de red virtual con RDID1 no pueden enviar paquetes para adaptadores de red virtual con RDID2 sin recorrer una puerta de enlace.  
  
> [!NOTE]  
> En la descripción de flujo de paquetes anterior, el término "máquina virtual" realmente significa el adaptador de red virtual en la máquina virtual. El caso común es que una máquina virtual solo tiene un adaptador de red virtual. En este caso, las palabras "virtual machine" y "adaptador de red virtual" conceptualmente pueden significan lo mismo.  
  
Cada subred virtual define una subred de IP de capa 3 y un límite de dominio de difusión de nivel 2 (L2) similar a una VLAN. Cuando una máquina virtual difunde un paquete, HNV usa unidifusión replicación (UR) para hacer una copia del paquete original y reemplaza la dirección IP de destino y MAC con las direcciones de cada máquina virtual que se encuentran en el mismo VSID.  
  
> [!NOTE]  
> Cuando se incluye Windows Server 2016, difusiones de difusión y de subred se implementará mediante la replicación unidifusión. Enrutamiento de multidifusión de subred de entre y IGMP no son compatibles.  
  
Además de ser un dominio de difusión, el VSID proporciona aislamiento. Un adaptador de red virtual en HNV está conectado a un puerto de conmutador de Hyper-V que tenga las reglas ACL que se aplican directamente al puerto (virtualNetworkInterface recurso REST) o a la subred virtual (VSID) del que es una parte.  
  
El puerto del conmutador de Hyper-V debe tener una regla de ACL que se aplica. Esta ACL podría ser permitir todo, denegar todo, o sea más específico para permitir que solo a determinados tipos de tráfico que se basa en la coincidencia de 5 tuplas (dirección IP de origen, dirección IP de destino, puerto de origen, el puerto de destino, protocolo).  
  
> [!NOTE]  
> Las extensiones del conmutador de Hyper-V no funcionará con HNVv2 en la nueva pila de Software definido de redes (SDN). HNVv2 se implementa mediante la extensión de cambiar de plataforma de filtrado Virtual (VFP) de Azure que no puede usarse junto con cualquier otra extensión de terceros 3 conmutador.  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Cambiar a otra y enrutamiento en la virtualización de la red de Hyper-V  
HNVv2 implementa cambio correcto de nivel 2 (L2) y la semántica de enrutamiento de nivel 3 (L3) para funcionan como un conmutador físico o enrutador funcionaría. Cuando una máquina virtual conectado a una red virtual de HNV intenta establecer una conexión con otra máquina virtual en la misma virtual subred (VSID) que primero debes obtener la dirección MAC de entidad emisora de certificados de la máquina virtual remota. Si hay una entrada de ARP para IP de dirección el destino de la máquina virtual en ARP tabla el origen de la máquina virtual, se usa la dirección MAC de esta entrada. Si no existe una entrada, la máquina virtual de origen enviará que difusión ARP con una solicitud para la dirección MAC correspondiente a la dirección IP de la máquina virtual de destino que va a devolver. El modificador de Hyper-V se interceptar esta solicitud y enviarlo al agente de Host. El agente de Host buscará en su base de datos local para una dirección MAC correspondiente para la dirección IP del equipo de destino solicitado virtual.  
  
> [!NOTE]  
> El agente de Host, que actúa como el servidor OVSDB, usa una variante del esquema VTEP para almacenar las asignaciones de CA-PA, tabla de MAC y así sucesivamente.  
  
Si una dirección MAC está disponible, el agente de Host inserta una respuesta ARP y se envía a la máquina virtual. Después de pila de red de la máquina virtual tenga toda la información de encabezado L2 necesaria, el marco se envía al puerto de Hyper-V correspondiente en el modificador V. Internamente, el modificador de Hyper-V prueba este marco de la tupla de N reglas coincidentes asignada al puerto-V y se aplica ciertas transformaciones para el fotograma según estas reglas. Lo más importante, se aplica un conjunto de transformaciones de encapsulación para construir el encabezado de encapsulación mediante NVGRE o VXLAN, según la directiva definida en el controlador de red. En función de la directiva de programar el agente de Host, una asignación PA de CA se utiliza para determinar la dirección IP del host Hyper-V donde se encuentra la máquina virtual de destino. El modificador de Hyper-V garantiza las reglas de enrutamiento correctas y etiquetas VLAN se aplican al paquete externo para llegar a la dirección remota de PA.  
  
Si una máquina virtual conectada a una red virtual de HNV quiere crear una conexión con una máquina virtual en una subred virtual diferentes (VSID), el paquete debe enrutarse según corresponda. HNV supone una topología de estrella donde hay solo una dirección IP en el espacio de CA utilizado como el próximo salto para llegar a todos los prefijos IP (significado una ruta o puerta de enlace predeterminada). Actualmente, esto impone una limitación de una única ruta predeterminada y no se admiten rutas no predeterminados.  
  
### <a name="routing-between-virtual-subnets"></a>Enrutamiento entre subredes Virtual  
En una red física, una subred IP es un dominio de nivel 2 (L2) donde equipos (virtuales o físicos) pueden comunicarse directamente con ellos. El dominio de L2 es un dominio donde se aprenden entradas ARP (mapa de dirección IP:MAC) a través de las solicitudes de ARP se emiten en todas las interfaces y respuestas ARP se envían al host solicitante de difusión. El equipo usa la información de MAC aprendida de la respuesta ARP para construir completamente el marco L2, incluidos los encabezados de Ethernet. Sin embargo, si es una dirección IP en una subred L3 diferente, la solicitud ARP no traspasa este límite L3. En su lugar, una interfaz de enrutador L3 (puerta de enlace salto o el valor predeterminado) con una dirección IP en la subred de origen debe responder a estas solicitudes ARP con su propia dirección MAC.  
  
En las redes de Windows estándar, un administrador puede crear rutas estáticas y asignar a una interfaz de red. Además, la "puerta de enlace predeterminada" normalmente está configurado para la dirección IP de salto siguiente en una interfaz que se envían los paquetes destinados a la ruta predeterminada (0.0.0.0/0). Los paquetes se envían a esta puerta de enlace predeterminada si no existen ningún rutas específicas. Suele ser el enrutador para la red física.  HNV usa un enrutador integrado que forma parte de todos los hosts y que tiene una interfaz en cada VSID para crear un enrutador distribuido para las redes virtuales.  
  
Dado que HNV se da por hecho una topología de estrella, el enrutador distribuido HNV actúa como una puerta de enlace único para todo el tráfico que se va entre subredes Virtual que forman parte de la misma red VSID. La dirección utiliza como predeterminados de puerta de enlace predeterminada a la dirección IP más bajo en la VSID y se asigna al enrutador HNV distribuido. Una manera muy eficaz para todo el tráfico dentro de una red VSID enrutarse correctamente porque cada host puede enrutar el tráfico directamente al host apropiado sin necesidad de un intermediario permite este enrutador distribuido.  Esto es especialmente cierto cuando dos máquinas virtuales en la misma máquina virtual red pero diferentes subredes Virtual se encuentran en el mismo host físico.  Como se verá más adelante en esta sección, el paquete nunca debe dejar el host físico.  
  
### <a name="routing-between-pa-subnets"></a>Enrutamiento entre subredes PA  
A diferencia de HNVv1 que asigna una dirección IP PA para cada subred Virtual (VSID), HNVv2 ahora usa una dirección IP de PA por miembros del equipo NIC Switch-Embedded agrupación (conjunto). La implementación predeterminada supone un equipo de dos NIC y asigna dos direcciones IP PA por host. Un único host tiene IPs PA asignado desde la misma subred lógica de proveedor (PA) en la misma VLAN. Dos máquinas virtuales de inquilino en la misma subred virtual efectivamente puede encontrarse en dos hosts diferentes que están conectados a dos subredes lógicas de otro proveedor. HNV construirá los encabezados IP externos para el paquete encapsulado en función de la asignación de CA-PA. Sin embargo, se basa en la pila TCP/IP de host a ARP para la puerta de enlace predeterminada PA y, a continuación, crea los encabezados de Ethernet externos en función de la respuesta ARP. Normalmente, esta respuesta ARP proviene de la interfaz SVI en el conmutador físico o enrutador L3 donde el host está conectado. HNV, por tanto, se basa en el enrutador L3 para el enrutamiento de los paquetes encapsulados entre subredes lógicas de proveedor / VLAN.  
  
### <a name="routing-outside-a-virtual-network"></a>Enrutamiento fuera de una red Virtual  
La mayoría de las implementaciones de cliente requerirá comunicación desde el entorno de HNV a los recursos que no forman parte del entorno de HNV. Puertas de enlace de virtualización de red son necesarias para permitir la comunicación entre los dos entornos. Infraestructuras que requieren una puerta de enlace HNV incluyen nube privada y nube híbrida. Básicamente, puertas de enlace HNV son necesarios para el enrutamiento de capa 3 entre redes (físicas) internas y externas (incluidos NAT) o entre distintos sitios o nubes (públicas o privadas) que utilizan un túnel IPSec VPN o GRE.  
  
Puertas de enlace pueden originarse en factores de forma física diferente. Se pueden crear en Windows Server 2016, incorpora un modificador de arriba de bastidor (términos de referencia) que actúa como una puerta de enlace VXLAN, accede a través de un IP Virtual (VIP) ha anunciado por un equilibrado de carga, poner en otros dispositivos de red existente, o puede ser un nuevo dispositivo de red independiente.  
  
Para obtener más información sobre las opciones de puerta de enlace de RAS de Windows, consulta [puerta de enlace de RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="packet-encapsulation"></a>Encapsulación de paquetes  
Cada adaptador de red virtual en HNV está asociado con dos direcciones IP:  
  
-   **Dirección de cliente** (CA) la dirección IP asignada por el cliente, en función de su infraestructura de intranet. Esta dirección permite al cliente intercambiar el tráfico de red con la máquina virtual como si hubiera no se ha movido a una nube pública o privada. La CA es visible para la máquina virtual y accesible para el cliente.  
  
-   **Dirección del proveedor** (PA) la dirección IP asignada por el proveedor de hospedaje o los administradores de los centros de datos en función de su infraestructura de red física. El PA aparece en los paquetes de la red que se intercambian con el servidor que ejecuta Hyper-V que hospeda la máquina virtual. El PA está visible en la red física, pero no a la máquina virtual.  
  
Las CA mantener la topología de red del cliente, que se virtualizan y desacoplada de la topología de red física subyacente real y direcciones, tal como se implementa por PAs. El siguiente diagrama muestra la relación conceptual entre máquina virtual entidades emisoras de certificados y la infraestructura de red PAs como resultado de la virtualización de la red.  
  
![Diagrama conceptual de la virtualización de la red a través de infraestructura física](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
Figura 6: Diagrama Conceptual de la virtualización de la red a través de infraestructura física  
  
En el diagrama, máquinas virtuales de cliente está enviando paquetes de datos en el espacio de la CA que recorren la infraestructura de red física a través de sus propios redes virtuales o "túneles". En el ejemplo anterior, los túneles se pueden considerar como "envolventes" alrededor de los paquetes de datos de Contoso y Fabrikam con etiquetas de envío verdes (direcciones PA) se envía desde el host de origen a la izquierda al host de destino a la derecha. La clave es cómo los hosts determinan las direcciones de envío de"" (PA) correspondiente a la Contoso la CA de Fabrikam, ¿cómo se coloca el "envolvente" alrededor de los paquetes, y cómo se pueden desempaquetar los paquetes y ofrecer a las máquinas virtuales de destino Contoso y Fabrikam correctamente los hosts de destino.  
  
Esta simple analogía resalta los aspectos clave de la virtualización de red:  
  
-   Cada entidad emisora de certificados de máquina virtual se asigna a un host físico PA. Puede que haya múltiples entidades emisoras asociado con la mismo PA  
  
-   Máquinas virtuales enviar paquetes de datos en los espacios de entidad emisora de certificados, que se colocan en un "sobre" junto con un par de origen y destino PA en función de la asignación.  
  
-   Las asignaciones de CA-PA deben permitir a los hosts diferenciar los paquetes para las máquinas virtuales de cliente diferente.  
  
Como resultado, el mecanismo de la virtualización de la red es virtualizar las direcciones de red utilizadas por las máquinas virtuales. El controlador de red es responsable de la asignación de dirección, y el agente de host mantiene la base de datos de asignación mediante el esquema MS_VTEP. La siguiente sección describe el mecanismo real de la virtualización de dirección.  
  
## <a name="network-virtualization-through-address-virtualization"></a>Virtualización de la red mediante la virtualización de dirección  
Implementa HNV superponer redes inquilino con red virtualización genérico enrutamiento encapsulación (NVGRE) o la red de área Local eXtensible Virtual (VXLAN).  VXLAN es el predeterminado.  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Red de área Local eXtensible virtual (VXLAN)  
Virtual eXtensible red de área Local (VXLAN) ([RFC 7348](http://www.rfc-editor.org/info/rfc7348)) se ha adoptado ampliamente protocolo en el mercado, con el soporte técnico de los proveedores como Cisco, Brocade, Arista, HP, Dell y otros. El protocolo VXLAN usa UDP como transporte. El puerto de destino asignado IANA UDP para VXLAN es 4789 y el puerto UDP de origen debe ser un hash de la información del interior de los paquetes que se usará para ECMP propagación. Después del encabezado UDP, un encabezado VXLAN se anexa al paquete que incluye un campo de 4 bytes reservado seguido de un campo de 3 bytes para el VXLAN red identificador (VNI) - VSID - seguido por otro campo de 1 byte reservado. Después del encabezado VXLAN, se anexa el marco de CA L2 original (sin el marco de la CA Ethernet FCS).  
  
![Encabezado de paquete VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulación de enrutamiento genérico (NVGRE)  
Este mecanismo de virtualización de la red usa el encapsulamiento de enrutamiento genérico (NVGRE) como parte de encabezado de túnel. En NVGRE, el paquete de la máquina virtual se encapsula dentro de otro paquete. El encabezado de este paquete nuevo tiene el origen adecuado PA direcciones IP y destino además el identificador de subred Virtual, que se almacena en el campo de encabezado de GRE, como se muestra en la figura 7.  
  
![Encapsulación NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
Figura 7: Virtualización de la red - encapsulación NVGRE  
  
El identificador de subred Virtual permite a los hosts identificar la máquina virtual de cliente para cualquier paquete concreto, aunque la PA y la entidad de certificación de los paquetes que se superponen. Esto permite que todas las máquinas virtuales en el mismo host para compartir un sola PA, como se muestra en la figura 7.  
  
Compartir la PA tiene un gran impacto en la escalabilidad de la red. Se puede reducir considerablemente el número de direcciones IP y MAC que debas aprender si la infraestructura de red. Por ejemplo, si cada host final tiene un promedio de 30 máquinas virtuales, el número de la dirección IP y direcciones MAC que deben aprender si la infraestructura de red se reduce por un factor de 30.com los identificadores de subred Virtual incrustado en los paquetes habilitar fácil correlación de los paquetes a los clientes reales.  
  
El PA compartir esquema para Windows Server 2012 R2 es un PA por VSID por host. Para Windows Server 2016 el esquema es un PA por miembros del equipo NIC.  
  
Con Windows Server 2016 y versiones posteriores, HNV totalmente compatible con NVGRE y VXLAN sin necesidad de personalizarlos; no se requiere actualización o comprar nuevo hardware de red como de los enrutadores, conmutadores o NIC (adaptadores de red). Esto es porque estos paquetes en el cable son normal paquetes IP en el espacio PA, que es compatible con la infraestructura de red de hoy en día.  Sin embargo obtener el mejor provecho del rendimiento admite NIC con los controladores más recientes que admiten la descarga de la tarea.  
  
## <a name="multi-tenant-deployment-example"></a>Ejemplo de implementación de varios inquilinos  
El siguiente diagrama muestra un ejemplo de implementación de dos clientes ubicado en un centro de datos de la nube con la relación de CA-PA definida por las directivas de red.  
  
![Ejemplo de implementación de varios inquilinos](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
Figura 8: Ejemplo de implementación de varios inquilinos  
  
Considera el ejemplo en la figura 8. Antes de pasar al proveedor de hospedaje compartida IaaS servicio:  
  
-   Contoso Corp se ejecutó un SQL Server (denominado **SQL**) en la dirección IP, dirección 10.1.1.11 y un servidor web (denominado **Web**) en la dirección IP 10.1.1.12, que usa su SQL Server para las transacciones de base de datos.  
  
-   Fabrikam Corp se ejecutó SQL Server, también llamada **SQL** y le asigna la dirección IP 10.1.1.11 y un servidor web, también llamada **Web** y también en la dirección IP 10.1.1.12, que usa su SQL Server para las transacciones de base de datos.  
  
Damos por hecho que el proveedor de servicio de hospedaje ha creado anteriormente la red lógica de proveedor (PA) a través del controlador de red para que se corresponda para su topología de red física. El controlador de red asigna dos direcciones IP PA de prefijo IP de la lógica de la subred donde los hosts están conectados. El controlador de red también indica la etiqueta VLAN adecuada para aplicar las direcciones IP.  
  
Con el controlador de red, Corp de Contoso y Fabrikam Corp, a continuación, crea su red virtual y subredes que están respaldadas por la red de la lógica de proveedor (PA) especificada por el proveedor de servicio de hospedaje. Corp de Contoso y Fabrikam Corp mover sus respectivos servidores SQL y servidores web para el proveedor de hospedaje mismo comparten servicio IaaS donde, por casualidad, se ejecutan los **SQL** máquinas virtuales en el Host de Hyper-V 1 y la **Web** máquinas virtuales de (IIS 7) en el Host de Hyper-V 2. Todas las máquinas virtuales mantener sus direcciones IP de intranet originales (sus CA).  
  
Ambas empresas se asignan el siguiente identificador de subred Virtual (VSID) el controlador de red como se indica a continuación.  El agente de Host en cada uno de los hosts de Hyper-V recibe las direcciones IP PA asignadas desde el controlador de red y crea dos vNICs de host PA compartimento de la red no predeterminados. Una interfaz de red se asigna a cada uno de estos vNICs host donde se le asigna la dirección IP PA tal como se muestra a continuación:  
  
-   Máquinas virtuales de Contoso Corp VSID y PAs: **VSID** es 5001, **SQL PA** es 192.168.1.10, **Web PA** es 192.168.2.20  
  
-   Máquinas virtuales de Fabrikam Corp VSID y PAs: **VSID** es 6001, **SQL PA** es 192.168.1.10, **Web PA** es 192.168.2.20  
  
El controlador de red sondea todas las directivas de red (incluida la asignación de CA-PA) para el agente de Host SDN que mantendrá la directiva en un almacén persistente (en las tablas de bases de datos OVSDB).  
  
Cuando la máquina virtual de Contoso Corp Web (10.1.1.12) en el Host de Hyper-V 2 crea una conexión TCP al servidor SQL en 10.1.1.11, ocurre lo siguiente:  
  
-   ARP de máquina virtual para el destino de la dirección MAC de 10.1.1.11  
  
-   La extensión VFP vSwitch intercepta este paquete y lo envía al agente de Host SDN  
  
-   El agente de Host SDN tenga un aspecto en su almacén de directivas de la dirección MAC de 10.1.1.11  
  
-   Si se encuentra un MAC, el agente de Host inserta una respuesta ARP volver a la máquina virtual  
  
-   Si no se encuentra un MAC, se envía ninguna respuesta y la entrada ARP en la máquina virtual para 10.1.1.11 está marcada inaccesible.  
  
-   La máquina virtual ahora crea un paquete TCP con los encabezados IP y Ethernet de entidad emisora de certificados correctos y lo envía al vSwitch  
  
-   Reenvío de extensión en el vSwitch VFP procesa este paquete a través de las capas VFP (que se describe a continuación) asignada al origen vSwitch puerto en el que el paquete se recibió y crea una nueva entrada de flujo en la tabla de flujo unificado VFP  
  
-   El motor VFP realiza la búsqueda de coincidencia o una tabla de flujo de la regla para cada capa (por ejemplo, el nivel de red virtual), en función de los encabezados IP y Ethernet.  
  
-   El asociado a la regla en el nivel de red virtual hace referencia a un espacio de asignación de CA-PA y lleva a cabo encapsulación.  
  
-   Se especifica el tipo de encapsulación (VXLAN o NVGRE) en la capa de VNet junto con la VSID.  
  
-   En el caso de encapsulación VXLAN, se crea un encabezado UDP exterior con la VSID de 5001 en el encabezado VXLAN.  
    Se crea un encabezado IP exterior con la dirección de PA de origen y destino asignada a las 2 de Host de Hyper-V (192.168.2.20) y 1 de Host de Hyper-V (192.168.1.10) respectivamente, en función de almacén de directivas del agente de Host SDN.  
  
-   Este paquete después fluye a la capa de enrutamiento PA en VFP.  
  
-   La capa de enrutamiento PA en VFP, el compartimento de la red se usa para el tráfico PA-espacio y un ID de referencia y que usarán la pila TCP/IP del host para reenviar el paquete PA a 1 de Host de Hyper-V correctamente.  
  
-   Al recibir el paquete encapsulado, Host de Hyper-V 1 recibe el paquete en el compartimento de la red PA y reenviarla a vSwitch.  
  
-   VFP procesa el paquete a través de sus capas VFP y crear una nueva entrada de flujo en la tabla de flujo unificado VFP.  
  
-   El motor VFP coincide con las reglas de ingres en el nivel de red virtual y quita los encabezados de Ethernet, IP y VXLAN del paquete encapsulado externo.  
  
-   El motor VFP reenvía el paquete al puerto vSwitch al que está conectado a la máquina virtual de destino.  
  
Un proceso similar para el tráfico entre el Corp Fabrikam **Web** y **SQL** máquinas virtuales usa la configuración de directiva HNV para la Corporation Fabrikam. Como resultado, con máquinas virtuales HNV, Fabrikam Corp y Contoso Corp interactuar como si estuvieran en sus intranets originales. Nunca pueden interactuar entre sí, aunque están usando las mismas direcciones IP.  
  
Las direcciones independientes (CA y PAs), la configuración de directiva de los hosts de Hyper-V y la traducción de direcciones entre la entidad de certificación y el PA para el tráfico entrante y saliente máquina virtual aislar estos conjuntos de servidores con la clave NVGRE o la VNID VLXAN. Además, las asignaciones de virtualización y transformación desacopla la arquitectura de red virtual de la infraestructura de red física. Aunque Contoso **SQL** y **Web** y Fabrikam **SQL** y **Web** residen en sus propios subredes de IP de entidad emisora de certificados (10.1.1/24), su implementación física se realiza en dos hosts de subredes PA diferentes, 192.168.1/24 y 192.168.2/24, respectivamente. La implicación es que entre subred aprovisionamiento y máquinas virtuales migración en vivo ser posibles con HNV.  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Arquitectura de virtualización de la red de Hyper-V  
En Windows Server 2016, HNVv2 se implementa mediante Azure Virtual filtrado de plataforma (VFP) que es una extensión de filtrado NDIS en el conmutador de Hyper-V. El concepto clave de VFP es que un motor de flujo de la acción de coincidencia con una API interna expuesta al agente SDN Host para la programación de directiva de red. El agente de Host SDN sí recibe la directiva de red desde el controlador de red sobre los canales de comunicación OVSDB y WCF SouthBound. No solo es la directiva de red virtual (por ejemplo, la asignación de CA-PA) programan mediante VFP pero directivas adicionales como ACL, QoS y así sucesivamente.  
  
La jerarquía de objetos para el vSwitch y VFP reenvío de extensión es la siguiente:  
  
-   vSwitch  
  
    -   Administración de NIC externa  
  
    -   Descarga de Hardware NIC  
  
    -   Reglas de reenvío globales  
  
    -   Puerto  
  
        -   Reenvío de capa para el anclaje de pelo de salida  
  
        -   Listas de espacio de asignaciones y conjuntos de NAT  
  
        -   Tabla de flujo unificado  
  
        -   Capa VFP  
  
            -   Tabla de flujo  
  
            -   Grupo  
  
            -   Regla  
  
                -   Las reglas pueden hacer referencia a espacios  
  
En la VFP, una capa se crea por tipo de directiva (por ejemplo, red Virtual) y es un conjunto genérico de tablas de regla y el flujo. No tiene ninguna funcionalidad intrínseca hasta que las reglas específicas se asignan a ese capa para implementar esta funcionalidad. Cada capa se asigna una prioridad y capas se asignan a un puerto ascendente prioridad. Las reglas se organizan en grupos que se basa principalmente en la dirección y la familia de direcciones IP. Grupos también se les asigna una prioridad y a lo sumo, una regla de un grupo puede coincidir con un determinado flujo.  
  
La lógica de reenvío para el vSwitch con extensión VFP es la siguiente:  
  
-   Procesamiento de entrada (entrada desde el punto de vista de paquete llegan a un puerto)  
  
-   Desvío de llamadas  
  
-   Procesamiento de salida (salida desde el punto de vista de salir de un puerto de paquete)  
  
VFP admite reenvío de MAC interno para tipos de encapsulación NVGRE y VXLAN, así como el desvío de llamadas exterior VLAN MAC en función.  
  
La extensión VFP tiene un acceso lento y rutas de acceso rápido de cruce seguro del paquete. El primer paquete en un flujo debe recorrer todos los grupos de reglas de cada capa y hacer una regla de búsqueda que es una operación costosa. Sin embargo, una vez que un flujo está registrado en la tabla de flujo unificada con una lista de las acciones (según las reglas coincidas) todos los paquetes siguientes se procesará en función de las entradas de la tabla de flujo unificado.  
  
Directiva HNV es programar el agente de host. Cada adaptador de red de la máquina virtual está configurado con una dirección IPv4. Estas son las entidades emisoras que usará las máquinas virtuales para comunicarse entre sí, y se ejecutan en los paquetes IP de las máquinas virtuales. HNV encapsula el marco de CA en un marco de PA en función de las directivas de virtualización de red almacenadas en la base de datos del agente de host.  
  
![Arquitectura HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
Figura 9: HNV arquitectura  
  
## <a name="summary"></a>Resumen  
En la nube centros de datos pueden proporcionar muchas ventajas, como aumentar la escalabilidad y mejor utilización de recursos. Para obtener estos beneficios posibles requiere una tecnología que fundamentalmente soluciona los problemas de escalabilidad de varios inquilino en un entorno dinámico. HNV se diseñó para solucionar estos problemas y también desacoplar la topología de red virtual para la topología de red física para mejorar la eficacia operativa del centro de datos. Crear en un estándar existente, HNV se ejecuta en el centro de datos de hoy y funciona con la infraestructura VXLAN existente. Los clientes con HNV pueden ahora consolidar sus centros de datos en una nube privada o perfectamente extender sus centros de datos al entorno de un proveedor de servidor host con una nube híbrida.  
  
## <a name="BKMK_LINKS"></a>Consulta también  
Para obtener más información sobre HNVv2 consulta los siguientes vínculos:  
  
|Tipo de contenido|Referencias|  
|----------------|--------------|  
|**Recursos de la Comunidad**|-   [Blog de arquitectura de nube privada](http://blogs.technet.com/b/privatecloud)<br />-Preguntas:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [Borrador NVGRE RFC](http://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologías relacionadas**|-Para los detalles técnicos de virtualización de la red de Hyper-V en Windows Server 2012 R2, consulta [detalles técnicos de virtualización de Hyper-V en red](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Controlador de red](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


