---
title: Detalles técnicos de la virtualización de red de Hyper-V en Windows Server 2016
description: En este tema se proporciona información técnica sobre la virtualización de red de Hyper-V en Windows Server 2016
manager: grcusanz
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 5341dfe03a85e192ce18baec44f5213a52a43fd7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952512"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Detalles técnicos de la virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server 2016

La virtualización de servidores permite que se ejecuten varias instancias de servidor al mismo tiempo en un solo host físico; aunque las instancias del servidor estén aisladas entre sí. Cada máquina virtual básicamente funciona como si fuera el único servidor que se ejecuta en el equipo físico.

La virtualización de red ofrece una funcionalidad similar, en la que varias redes virtuales (posiblemente con direcciones IP superpuestas) se ejecutan en la misma infraestructura de red física y cada red virtual funciona como si se tratase de la única red virtual que se ejecuta en la infraestructura de red compartida. La Figura 1 muestra esta relación.

![Virtualización de servidor frente a virtualización de red](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)

Figura 1: Virtualización de servidor frente a virtualización de red

## <a name="hyper-v-network-virtualization-concepts"></a>Conceptos de la virtualización de red de Hyper-V
En virtualización de red de Hyper-V (HNV), un cliente o un inquilino se define como el "propietario" de un conjunto de subredes IP que se implementan en un centro de información empresarial o de centros de recursos. Un cliente puede ser una corporación o una empresa con varios departamentos o unidades de negocio en un centro de datos privado que requiere aislamiento de red o un inquilino en un centro de datos público que está hospedado por un proveedor de servicios. Cada cliente puede tener una o más [redes virtuales](#VirtualNetworks) en el centro de información y cada red virtual se compone de una o más [subredes virtuales](#VirtualSubnets).

Hay dos implementaciones de HNV que estarán disponibles en Windows Server 2016: HNVv1 y HNVv2.

-   **HNVv1**

    HNVv1 es compatible con Windows Server 2012 R2 y System Center 2012 R2 Virtual Machine Manager (VMM). La configuración de HNVv1 se basa en los cmdlets de Windows PowerShell y la administración de WMI (facilitado a través de System Center VMM) para definir la configuración de aislamiento y las asignaciones y el enrutamiento de la red virtual a la dirección física (PA). No se han agregado características adicionales a HNVv1 en Windows Server 2016 y no se han planeado nuevas características.

    *   La plataforma establece la formación de equipos y HNV v1 no es compatible.

    o para usar NVGRE puertas de enlace de alta disponibilidad, los usuarios deben usar el equipo LBFO o ningún equipo. Or

    o use puertas de enlace implementadas por la controladora de red con el conmutador establecido en equipo.


-   **HNVv2**

    En HNVv2 se incluye un número significativo de nuevas características que se implementan mediante la extensión de reenvío de la plataforma de filtrado virtual de Azure (VFP) en el conmutador de Hyper-V. HNVv2 está totalmente integrado con Microsoft Azure Stack que incluye la nueva controladora de red en la pila de redes definidas por software (SDN).  La Directiva de red virtual se define a través de la [controladora de red](../../../sdn/technologies/network-controller/Network-Controller.md) de Microsoft mediante una API de RESTful NORTHBOUND (NB) y se sondea a un agente de host a través de varios SouthBound INTEFACES (SBI), incluido OVSDB. La Directiva de programas del agente de host de la extensión VFP del conmutador de Hyper-V donde se aplica.

    > [!IMPORTANT]
    > Este tema se centra en HNVv2.

### <a name="virtual-network"></a><a name="VirtualNetworks"></a>Virtual Network

-   Cada red virtual se compone de una o más subredes virtuales. Una red virtual forma un límite de aislamiento donde las máquinas virtuales dentro de una red virtual solo pueden comunicarse entre sí. Tradicionalmente, este aislamiento se aplicó mediante VLAN con un intervalo de direcciones IP segregado y una etiqueta 802.1 q o ID. de VLAN. Pero con HNV, el aislamiento se aplica mediante la encapsulación NVGRE o VXLAN para crear redes de superposición con la posibilidad de que se superpongan subredes IP entre los clientes o los inquilinos.

-   Cada red virtual tiene un identificador de dominio de enrutamiento único (RDID) en el host. Este RDID se asigna aproximadamente a un identificador de recurso para identificar el recurso de REST de red virtual en el controlador de red. Se hace referencia al recurso REST de red virtual mediante un espacio de nombres de identificador uniforme de recursos (URI) con el identificador de recurso anexado.

### <a name="virtual-subnets"></a><a name="VirtualSubnets"></a>Subredes virtuales

-   Una subred virtual implementa la semántica de la subred IP de capa 3 para las máquinas virtuales de la misma subred virtual. La subred virtual forma un dominio de difusión (similar a una VLAN) y el aislamiento se aplica mediante el campo Identificador de red de inquilino de NVGRE (TNI) o identificador de red de VXLAN (VNI).

-   Cada subred virtual pertenece a una sola red virtual (RDID) y se le asigna un identificador único de subred virtual (debajo) mediante la clave TNI o VNI en el encabezado de paquete encapsulado. El grupo de recursos debe ser único en el centro de bits y se encuentra en el intervalo de 4096 a 2 ^ 24-2.

Una ventaja clave de la red virtual y el dominio de enrutamiento es que permite a los clientes traer sus propias topologías de red (por ejemplo, subredes IP) a la nube. La Figura 2 muestra un ejemplo donde Contoso Corp cuenta con dos redes diferentes: Red de I+D y Red de ventas. Dado que estas redes cuentan con identificadores de dominio de enrutamiento diferentes, no pueden interactuar entre sí. Es decir, Red I+D de Contoso está aislada de Red de ventas de Contoso aunque ambas pertenezcan a Contoso Corp. Red I+D de Contoso contiene tres subredes virtuales. Tenga en cuenta que el RDID y el VSID son únicos dentro de un centro de datos.

![Redes de cliente y subredes virtuales](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)

Figura 2: Redes del cliente y subredes virtuales

**Reenvío de capa 2**

En la figura 2, las máquinas virtuales de la versión 1 a 5001 pueden tener sus paquetes reenviados a las máquinas virtuales que también se encuentran en el parámetro de la 5001 a través del conmutador de Hyper-V. Los paquetes entrantes de una máquina virtual en un solo 5001 se envían a un VPort específico en el conmutador de Hyper-V. El conmutador de Hyper-V de estos paquetes aplica las reglas de entrada (por ejemplo, encap) y las asignaciones (por ejemplo, el encabezado de encapsulación). Los paquetes se reenvían a un VPort diferente en el conmutador de Hyper-V (si la máquina virtual de destino está conectada al mismo host) o a otro conmutador de Hyper-V en otro host (si la máquina virtual de destino se encuentra en un host diferente).

**Enrutamiento de nivel 3**

Del mismo modo, las máquinas virtuales de un solo 5001 pueden tener sus paquetes enrutados a las máquinas virtuales en los parámetros 5003 5002 de el enrutador distribuido de HNV, que están presentes en cada VSwitch del host de Hyper-V. Tras entregar el paquete al conmutador de Hyper-V, HNV actualiza el ID. de actualización del paquete entrante en el ID. de la máquina virtual de destino. Esto solo sucederá si ambos VSID se encuentran en el mismo RDID.  Por lo tanto, los adaptadores de red virtual con RDID1 no pueden enviar paquetes a los adaptadores de red virtuales con RDID2 sin atravesar una puerta de enlace.

> [!NOTE]
> En la descripción anterior del flujo de paquetes, el término "máquina virtual" significa realmente el adaptador de red virtual de la máquina virtual. El caso más común es que una máquina virtual contenga un solo adaptador de red virtual. En este caso, las palabras "máquina virtual" y "adaptador de red virtual" pueden significar conceptualmente lo mismo.

Cada subred virtual define una subred de IP de capa 3 y un límite de dominio de difusión (L2) de capa 2 similar a una VLAN. Cuando una máquina virtual difunde un paquete, HNV usa la replicación de unidifusión (UR) para hacer una copia del paquete original y reemplazar la dirección IP de destino y el equipo MAC por las direcciones de cada máquina virtual que se encuentran en el mismo Id. de dispositivo.

> [!NOTE]
> Cuando se distribuye Windows Server 2016, las multidifusión y la subred se implementarán mediante la replicación de unidifusión. No se admite el enrutamiento de multidifusión entre subredes y IGMP.

Además de ser un dominio de difusión, el VSID proporciona aislamiento. Un adaptador de red virtual de HNV está conectado a un puerto de conmutador de Hyper-V que tendrá reglas de ACL aplicadas directamente al puerto (recurso de REST de virtualNetworkInterface) o a la subred virtual (debajo) de la que forma parte.

El puerto del conmutador de Hyper-V debe tener una regla de ACL aplicada. Esta ACL podría permitir todo, denegar todo o ser más específica para permitir solo determinados tipos de tráfico basados en la coincidencia de 5-tupla (IP de origen, IP de destino, Puerto de origen, Puerto de destino, protocolo).

> [!NOTE]
> Las extensiones de conmutador de Hyper-V no funcionarán con HNVv2 en la nueva pila de redes definidas por software (SDN). HNVv2 se implementa mediante la extensión de conmutador de la plataforma de filtrado virtual de Azure (VFP) que no se puede usar junto con ninguna otra extensión de conmutador de terceros.

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Conmutación y enrutamiento en virtualización de red de Hyper-V
HNVv2 implementa la semántica correcta de enrutamiento de nivel 2 (L2) y de enrutamiento de capa 3 (L3) para funcionar como un conmutador físico o un enrutador. Cuando una máquina virtual conectada a una red virtual de HNV intenta establecer una conexión con otra máquina virtual en la misma subred virtual (es decir,), primero deberá conocer la dirección MAC de la CA de la máquina virtual remota. Si hay una entrada ARP para la dirección IP de la máquina virtual de destino en la tabla ARP de la máquina virtual de origen, se usa la dirección MAC de esta entrada. Si no existe una entrada, la máquina virtual de origen enviará una difusión ARP con una solicitud de la dirección MAC correspondiente a la dirección IP de la máquina virtual de destino que se va a devolver. El conmutador de Hyper-V interceptará esta solicitud y la enviará al agente de host. El agente de host buscará en su base de datos local la dirección MAC correspondiente a la dirección IP de la máquina virtual de destino solicitada.

> [!NOTE]
> El agente de host, que actúa como servidor de OVSDB, utiliza una variante del esquema VTEP para almacenar asignaciones de CA-PA, tabla MAC, etc.

Si hay una dirección MAC disponible, el agente de host inserta una respuesta ARP y la devuelve a la máquina virtual. Una vez que la pila de red de la máquina virtual tiene toda la información de encabezado L2 necesaria, el marco se envía al puerto de Hyper-V correspondiente en el conmutador V. Internamente, el conmutador de Hyper-V prueba este fotograma con las reglas de coincidencia de N-tupla asignadas al puerto V y aplica ciertas transformaciones al marco en función de estas reglas. Lo más importante es que se aplique un conjunto de transformaciones de encapsulación para construir el encabezado de encapsulación mediante NVGRE o VXLAN, en función de la Directiva definida en el controlador de red. En función de la Directiva programada por el agente de host, se usa una asignación de CA-PA para determinar la dirección IP del host de Hyper-V donde reside la máquina virtual de destino. El conmutador de Hyper-V garantiza que las reglas de enrutamiento correctas y las etiquetas VLAN se aplican al paquete externo para que alcance la dirección PA remota.

Si una máquina virtual conectada a una red virtual de HNV desea crear una conexión con una máquina virtual en una subred virtual diferente (un ID. de equipo), el paquete debe enrutarse en consecuencia. HNV supone una topología de estrella en la que solo hay una dirección IP en el espacio de la CA que se usa como próximo salto para llegar a todos los prefijos IP (es decir, una puerta de enlace/ruta predeterminada). Actualmente, esto exige una limitación a una única ruta predeterminada y no se admiten las rutas no predeterminadas.

### <a name="routing-between-virtual-subnets"></a>Enrutamiento entre subredes virtuales
En una red física, una subred IP es un dominio de nivel 2 (L2) en el que los equipos (virtuales y físicos) pueden comunicarse directamente entre sí. El dominio L2 es un dominio de difusión en el que las entradas ARP (IP: asignación de direcciones MAC) se aprenden mediante solicitudes ARP que se difunden en todas las interfaces y las respuestas ARP se envían al host que realiza la solicitud. El equipo usa la información de MAC obtenida de la respuesta ARP para construir completamente el marco L2, incluidos los encabezados Ethernet. Sin embargo, si una dirección IP se encuentra en una subred L3 diferente, la solicitud ARP no cruza este límite L3. En su lugar, una interfaz de enrutador L3 (puerta de enlace de próximo salto o predeterminada) con una dirección IP en la subred de origen debe responder a estas solicitudes ARP con su propia dirección MAC.

En las redes de Windows estándar, un administrador puede crear rutas estáticas y asignarlas a una interfaz de red. Además, una "puerta de enlace predeterminada" se configura normalmente para que sea la dirección IP de próximo salto en una interfaz en la que se envían los paquetes destinados a la ruta predeterminada (0.0.0.0/0). Los paquetes se envían a esta puerta de enlace predeterminada si no existen rutas específicas. Normalmente se trata del enrutador de la red física.  HNV usa un enrutador integrado que forma parte de todos los hosts y tiene una interfaz en cada elemento de error para crear un enrutador distribuido para las redes virtuales.

Dado que HNV supone una topología en estrella, el enrutador distribuido HNV actúa como puerta de enlace predeterminada única para todo el tráfico que se encuentra entre las subredes virtuales que forman parte de la misma red de los mismos. La dirección que se usa como la puerta de enlace predeterminada tiene como valor predeterminado la dirección IP más baja en la asignación de direcciones y está asignada al enrutador distribuido HNV. Este enrutador distribuido permite enrutar de forma muy eficaz todo el tráfico dentro de una red de este tipo, ya que cada host puede enrutar directamente el tráfico al host adecuado sin necesidad de un intermediario.  Esto es especialmente cierto cuando dos máquinas virtuales que se encuentran en la misma subred de VM, pero en diferentes subredes virtuales, están en el mismo host físico.  Como verá más adelante en esta sección, el paquete nunca debe abandonar el host físico.

### <a name="routing-between-pa-subnets"></a>Enrutamiento entre subredes PA
A diferencia de HNVv1, que asignaba una dirección IP PA para cada subred virtual (el usuario 1), HNVv2 ahora usa una dirección IP PA por miembro del equipo NIC de conmutador incrustado en equipo (SET). La implementación predeterminada presupone un equipo con dos NIC y asigna dos direcciones IP PA por host. Un solo host tiene direcciones IP de PA asignadas de la misma subred lógica de proveedor (PA) en la misma VLAN. En realidad, es posible que dos máquinas virtuales de inquilino de la misma subred virtual se encuentren en dos hosts diferentes que estén conectados a dos subredes lógicas de proveedor diferentes. HNV creará los encabezados IP externos para el paquete encapsulado basado en la asignación de CA-PA. Sin embargo, se basa en la pila TCP/IP del host para ARP para la puerta de enlace PA predeterminada y, a continuación, crea los encabezados Ethernet externos basados en la respuesta ARP. Normalmente, esta respuesta ARP proviene de la interfaz SVI en el conmutador físico o enrutador L3 en el que el host está conectado. Por tanto, HNV se basa en el enrutador L3 para enrutar los paquetes encapsulados entre las subredes lógicas del proveedor/VLAN.

### <a name="routing-outside-a-virtual-network"></a>Enrutamiento fuera de una red virtual
La mayoría de las implementaciones de clientes requerirán comunicación desde el entorno de HNV a los recursos que no forman parte de dicho entorno. Se requieren puertas de enlace de Virtualización de red para permitir la comunicación entre los dos entornos. Las infraestructuras que requieren una puerta de enlace de HNV incluyen la nube privada y la nube híbrida. Básicamente, las puertas de enlace de HNV son necesarias para el enrutamiento de capa 3 entre redes internas y externas (incluido NAT) o entre diferentes sitios o nubes (privado o público) que usan un túnel de VPN o GRE de IPSec.

Las puertas de enlace pueden venir en diferentes factores de forma físicos. Pueden compilarse en Windows Server 2016, incorporarse a un conmutador de parte superior del rack (TOR) que actúa como puerta de enlace de VXLAN, a la que se accede a través de una IP virtual (VIP) anunciada por un equilibrador de carga, se coloca en otros dispositivos de red existentes, o bien puede ser un nuevo dispositivo de red independiente.

Para obtener más información acerca de las opciones de puerta de enlace RAS de Windows, consulte [puerta de enlace ras](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).

## <a name="packet-encapsulation"></a>Encapsulación de paquetes
Cada adaptador de red virtual en HNV está asociado a dos direcciones IP:

-   **Dirección de cliente** (CA) la dirección IP asignada por el cliente, en función de la infraestructura de la intranet. Esta dirección permite al cliente intercambiar tráfico de red con la máquina virtual como si no se hubiera migrado a una nube pública o privada. La CA está visible para la máquina virtual y el cliente puede obtener acceso a ella.

-   **Dirección de proveedor** (PA) la dirección IP asignada por el proveedor de hospedaje o los administradores de centros de recursos en función de su infraestructura de red física. La PA aparece en los paquetes de la red que se intercambian con el servidor que ejecuta Hyper-V que hospeda la máquina virtual. La PA está visible en la red física, pero no lo está para la máquina virtual.

Las CA mantienen la topología de red del cliente, que está virtualizada y desacoplada de la topología y las direcciones de red física subyacente real, tal como lo implementan las PA. En el siguiente diagrama se muestra la relación conceptual entre las CA de máquinas virtuales y las PA de infraestructura de red como resultado de la virtualización de red.

![Diagrama conceptual de virtualización de red a través de una infraestructura física](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)

Figura 6: Diagrama conceptual de virtualización de red a través de una infraestructura física

En el diagrama, las máquinas virtuales del cliente envían paquetes de datos en el espacio de la CA, que atraviesan la infraestructura de red física a través de sus propias redes virtuales o "túneles". En el ejemplo anterior, los túneles pueden considerarse como "sobres" alrededor de los paquetes de datos de Contoso y Fabrikam con etiquetas verdes de envío (direcciones PA) que se entregan desde el host de origen de la izquierda al host de destino a la derecha. La clave es la forma en que los hosts determinan las "direcciones de envío" (PA) correspondientes a las CA de Contoso y Fabrikam, cómo se coloca el "sobre" alrededor de los paquetes y cómo los hosts de destino pueden desencapsular los paquetes y entregarlos correctamente a las máquinas virtuales de destino de Contoso y fabrikam.

Esta simple comparación destacó los aspectos clave de la virtualización de red:

-   La CA de cada máquina virtual se asigna a una PA de host físico. Puede haber varios CA asociados a la misma PA.

-   Las máquinas virtuales envían paquetes de datos en los espacios de CA, que se colocan en un "sobre" con un par de origen y destino de PA basado en la asignación.

-   Las asignaciones de CA-PA deben permitir a los hosts diferenciar paquetes de diferentes equipos virtuales del cliente.

Como resultado, el mecanismo para virtualizar la red es virtualizar las direcciones de red que utilizan las máquinas virtuales. La controladora de red es responsable de la asignación de direcciones y el agente de host mantiene la base de datos de asignación mediante el esquema de MS_VTEP. En la próxima sección se describe el mecanismo real de virtualización de direcciones.

## <a name="network-virtualization-through-address-virtualization"></a>Virtualización de red a través de virtualización de direcciones
HNV implementa redes de inquilinos superpuesto mediante encapsulación de enrutamiento genérico de virtualización de red (NVGRE) o la red de área local extensible (VXLAN).  VXLAN es el valor predeterminado.

### <a name="virtual-extensible-local-area-network-vxlan"></a>Red de área local extensible virtual (VXLAN)
El protocolo de red de área local extensible (VXLAN) ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) se ha adoptado ampliamente en el mercado, con soporte técnico de proveedores como Cisco, Brocade, arista, Dell, HP y otros. El protocolo VXLAN utiliza UDP como transporte. El puerto de destino UDP asignado por IANA para VXLAN es 4789 y el puerto de origen UDP debe ser un hash de información del paquete interno que se va a usar para la propagación de ECMP. Después del encabezado UDP, se anexa un encabezado VXLAN al paquete, que incluye un campo de 4 bytes reservado seguido de un campo de 3 bytes para el identificador de red VXLAN (VNI)-debajo, seguido de otro campo de 1 byte reservado. Después del encabezado VXLAN, se anexa el marco de la CA de nivel 2 original (sin el fotograma Ethernet de CA FCS).

![Encabezado de paquete VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)

### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulación de enrutamiento genérico (NVGRE)
Este mecanismo de virtualización de red usa la encapsulación de enrutamiento genérico (NVGRE) como parte del encabezado de túnel. En NVGRE, el paquete de la máquina virtual se encapsula dentro de otro paquete. El encabezado de este nuevo paquete tiene las direcciones IP PA de origen y de destino apropiadas, además del identificador de subred virtual, que se almacena en el campo Clave del encabezado GRE, como se muestra en la Figura 7.

![Encapsulación NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)

Figura 7: Virtualización de red - Encapsulación NVGRE

El identificador de subred virtual permite a los hosts identificar la máquina virtual del cliente para cualquier paquete dado, aunque las PA y las CA de los paquetes puedan superponerse. Esto permite a las máquinas virtuales del mismo host compartir una sola PA, como se muestra en la Figura 7.

Compartir la PA tiene un gran impacto en la escalabilidad de la red. La cantidad de direcciones IP y MAC que la infraestructura de red necesita detectar puede reducirse sustancialmente. Por ejemplo, si cada host extremo tiene un promedio de 30 máquinas virtuales, la cantidad de direcciones IP y MAC que debe detectar la infraestructura de red se reduce en un factor de 30. Los identificadores de subred virtual insertados en los paquetes también permiten la fácil correlación de paquetes con los clientes reales.

El esquema de uso compartido de PA para Windows Server 2012 R2 es un PA por cada ID. de host. En Windows Server 2016, el esquema es un PA por miembro del equipo NIC.

Con Windows Server 2016 y versiones posteriores, HNV es totalmente compatible con NVGRE y VXLAN de forma integrada; NO requiere la actualización ni la compra de nuevo hardware de red como NIC (adaptadores de red), conmutadores o enrutadores. Esto se debe a que estos paquetes de la conexión son paquetes IP normales en el espacio de PA, que es compatible con la infraestructura de red de hoy.  Sin embargo, para obtener el mejor rendimiento, use NIC compatibles con los controladores más recientes que admiten descargas de tareas.

## <a name="multi-tenant-deployment-example"></a>Ejemplo de implementación multiinquilino
En el diagrama siguiente se muestra un ejemplo de implementación de dos clientes ubicados en un centro de usuarios de nube con la relación de CA-PA definida por las directivas de red.

![Ejemplo de implementación multiinquilino](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)

Figura 8: Ejemplo de implementación multiempresa

Considere el ejemplo en la Figura 8. Antes de pasar al servicio IaaS compartido del proveedor de hospedaje:

-   Contoso Corp ejecutó un servidor SQL Server (llamado **SQL**) en la dirección IP 10.1.1.11 y un servidor web (llamado **Web**) en la dirección IP 10.1.1.12, que usa SQL Server para transacciones de bases de datos.

-   Fabrikam Corp ejecutó un servidor SQL Server, también llamado **SQL** y asignó la dirección IP 10.1.1.11 y un servidor web, también llamado **Web** y también la dirección IP 10.1.1.12 que usa SQL Server para transacciones de bases de datos.

Supondremos que el proveedor de servicios de hosting ha creado previamente la red lógica del proveedor (PA) a través de la controladora de red para que se corresponda con su topología de red física. La controladora de red asigna dos direcciones IP PA del prefijo IP de la subred lógica donde se conectan los hosts. La controladora de red también indica la etiqueta VLAN adecuada para aplicar las direcciones IP.

Con la controladora de red, Contoso Corp y Fabrikam Corp crearán su red virtual y las subredes que están respaldadas por la red lógica del proveedor (PA) especificada por el proveedor de servicios de hosting. Contoso Corp y Fabrikam Corp mueven sus respectivos servidores SQL Server y sus servidores web al mismo servicio IaaS compartido del proveedor de hospedaje donde, casualmente, ejecutan las máquinas virtuales **SQL** en el host 1 de Hyper-V y las máquinas virtuales (IIS7) **Web** en el host 2 de Hyper-V. Todas las máquinas virtuales conservan las direcciones IP de intranet originales (sus CA).

A ambas empresas se les asigna el siguiente identificador de subred virtual (debajo) mediante la controladora de red, como se indica a continuación.  El agente de host de cada uno de los hosts de Hyper-V recibe las direcciones IP de PA asignadas de la controladora de red y crea dos VNIC de host de PA en un compartimiento de red no predeterminado. Se asigna una interfaz de red a cada uno de estos hosts VNIC donde se asigna la dirección IP de PA, como se muestra a continuación:

-   Las máquinas virtuales de Contoso Corp y **PAS: no** se 5001, **SQL PA** es 192.168.1.10, **Web PA** es 192.168.2.20

-   Las máquinas virtuales de Fabrikam Corp y **PAS: no** se 6001, **SQL PA** es 192.168.1.10, **Web PA** es 192.168.2.20

La controladora de red sondea todas las directivas de red (incluida la asignación de CA-PA) con el agente de host de SDN que mantendrá la Directiva en un almacén persistente (en las tablas de base de datos OVSDB).

Cuando la máquina virtual web de Contoso Corp (10.1.1.12) en el host 2 de Hyper-V crea una conexión TCP al SQL Server en 10.1.1.11, ocurre lo siguiente:

-   ARP de VM para la dirección MAC de destino de 10.1.1.11

-   La extensión VFP del vSwitch intercepta este paquete y lo envía al agente de host de SDN.

-   El agente de host de SDN busca en su almacén de directivas la dirección MAC para 10.1.1.11

-   Si se encuentra un equipo MAC, el agente de host inserta una respuesta ARP de nuevo en la máquina virtual.

-   Si no se encuentra un equipo MAC, no se envía ninguna respuesta y la entrada ARP en la máquina virtual para 10.1.1.11 se marca como no accesible.

-   La máquina virtual ahora construye un paquete TCP con los encabezados IP y Ethernet de CA correctos y lo envía al vSwitch

-   La extensión de reenvío de VFP en vSwitch procesa este paquete a través de los niveles de VFP (que se describen a continuación) asignados al puerto vSwitch de origen en el que se recibió el paquete y crea una nueva entrada de flujo en la tabla de flujo unificada de VFP.

-   El motor VFP realiza la búsqueda de coincidencias de reglas o de tablas de flujo para cada capa (por ejemplo, la capa de red virtual) en función de los encabezados IP y Ethernet.

-   La regla coincidente en el nivel de red virtual hace referencia a un espacio de asignación de CA-PA y realiza la encapsulación.

-   El tipo de encapsulación (VXLAN o NVGRE) se especifica en el nivel de red virtual junto con el de la máquina virtual.

-   En el caso de la encapsulación VXLAN, se crea un encabezado UDP externo con el argumento de la 5001 en el encabezado VXLAN.
    Un encabezado IP externo se construye con la dirección PA de origen y de destino asignada al host 2 de Hyper-V (192.168.2.20) y al host 1 (192.168.1.10) de Hyper-V, respectivamente, basado en el almacén de directivas del agente de host de SDN.

-   A continuación, este paquete fluye a la capa de enrutamiento de PA en VFP.

-   La capa de enrutamiento de PA en VFP hará referencia al compartimiento de red que se usa para el tráfico de espacio de PA y un identificador de VLAN, y usa la pila TCP/IP del host para reenviar correctamente el paquete de PA al host 1 de Hyper-V.

-   Tras la recepción del paquete encapsulado, el host 1 de Hyper-V recibe el paquete en el compartimiento de red PA y lo reenvía al vSwitch.

-   El VFP procesa el paquete a través de sus capas de VFP y crea una nueva entrada de flujo en la tabla de flujo unificado de VFP.

-   El motor VFP coincide con las reglas de Ingres en el nivel de red virtual y elimina los encabezados Ethernet, IP y VXLAN del paquete encapsulado externo.

-   A continuación, el motor VFP reenvía el paquete al puerto vSwitch al que está conectada la máquina virtual de destino.

Un proceso similar para el tráfico entre las máquinas virtuales de Fabrikam Corp **Web** y **SQL** usa la configuración de la directiva de HNV de Fabrikam Corp. Como resultado, con HNV, las máquinas virtuales de Fabrikam Corp y Contoso Corp interactúan como si estuviesen en las intranet originales. Nunca pueden interactuar entre sí aunque usen las mismas direcciones IP.

Las direcciones independientes (CAs y PAs), la configuración de directiva de los hosts de Hyper-V y la traducción de direcciones entre la CA y la PA para el tráfico de la máquina virtual entrante y saliente aíslan estos conjuntos de servidores mediante la clave NVGRE o VNID VLXAN. Además, las asignaciones de virtualización y la transformación desacoplan la arquitectura de la red virtual de la infraestructura de la red física. Si bien Contoso **SQL** y **Web** y Fabrikam **SQL** y **Web** residen en sus propias subredes IP CA (10.1.1/24), las implementaciones físicas se producen en dos hosts de diferentes subredes PA, 192.168.1/24 y 192.168.2/24, respectivamente. La implicación es que el aprovisionamiento de máquinas virtuales entre subredes y la migración en vivo son posibles con HNV.

## <a name="hyper-v-network-virtualization-architecture"></a>Arquitectura de Virtualización de red de Hyper-V
En Windows Server 2016, HNVv2 se implementa mediante la plataforma de filtrado virtual (VFP) de Azure, que es una extensión de filtrado de NDIS en el conmutador de Hyper-V. El concepto clave de VFP es el de un motor de flujo de acción de coincidencia con una API interna expuesta al agente de host de SDN para la programación de la Directiva de red. El agente de host de SDN recibe una directiva de red del controlador de red a través de los canales de comunicación de OVSDB y WCF SouthBound. No solo es una directiva de red virtual (por ejemplo, asignación de CA-PA) programada con VFP, sino una directiva adicional como ACL, QoS, etc.

La jerarquía de objetos para el vSwitch y la extensión de reenvío de VFP es la siguiente:

-   vSwitch

    -   Administración de NIC externa

    -   Descargas de hardware NIC

    -   Reglas de reenvío global

    -   Port

        -   Capa de reenvío de salida para el anclaje de pelo

        -   Listas de espacio para asignaciones y grupos NAT

        -   Tabla de flujo unificada

        -   Capa de VFP

            -   Tabla Flow

            -   Grupo

            -   Regla

                -   Las reglas pueden hacer referencia a espacios

En el VFP, se crea una capa por cada tipo de directiva (por ejemplo, Virtual Network) y es un conjunto genérico de tablas de reglas y flujos. No tiene ninguna funcionalidad intrínseca hasta que se asignen reglas específicas a esa capa para implementar dicha funcionalidad. A cada capa se le asigna una prioridad y las capas se asignan a un puerto mediante prioridad ascendente. Las reglas se organizan en grupos basándose principalmente en la dirección y la familia de direcciones IP. A los grupos también se les asigna una prioridad y, como máximo, una regla de un grupo puede coincidir con un flujo determinado.

La lógica de reenvío para el vSwitch con la extensión VFP es la siguiente:

-   Procesamiento de entrada (entrada desde el punto de vista de un paquete que llega a un puerto)

-   Reenvío

-   Procesamiento de salida (salida desde el punto de vista del paquete que sale de un puerto)

VFP admite el reenvío de MAC interno para los tipos de encapsulación NVGRE y VXLAN, así como el reenvío externo basado en MAC VLAN.

La extensión VFP tiene una ruta de acceso lenta y una ruta de acceso rápida para el recorrido de paquetes. El primer paquete de un flujo debe atravesar todos los grupos de reglas de cada capa y realizar una búsqueda de reglas que es una operación costosa. Sin embargo, una vez que se registra un flujo en la tabla de flujo unificado con una lista de acciones (según las reglas que coinciden), todos los paquetes posteriores se procesarán en función de las entradas de la tabla de flujo unificada.

El agente de host programa la Directiva HNV. Cada adaptador de red de máquina virtual se configura con una dirección IPv4. Estas son las CA que utilizarán las máquinas virtuales para comunicarse entre sí y se pueden transportar en los paquetes IP desde las máquinas virtuales. HNV encapsula la trama de CA en un marco de PA en función de las directivas de virtualización de red almacenadas en la base de datos del agente de host.

![Arquitectura HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)

Figura 9: Arquitectura de HNV

## <a name="summary"></a>Resumen
Los centros de datos basados en la nube pueden proporcionar muchos beneficios como mayor escalabilidad y mejor utilización de los recursos. Advertir estos beneficios potenciales requiere una tecnología que fundamentalmente aborde los problemas de escalabilidad multiempresa en un entorno dinámico. HNV se diseñó para abordar estos problemas y también mejorar la eficacia operativa del centro de datos al desacoplar la topología de red virtual para la topología de red física. Basándose en un estándar existente, HNV se ejecuta en el centro de información de hoy en día y funciona con la infraestructura VXLAN existente. Ahora, los clientes con HNV pueden consolidar sus centros de recursos en una nube privada o ampliar sin problemas sus centros de recursos al entorno de un proveedor de servidores de hospedaje con una nube híbrida.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vea también
Para obtener más información sobre HNVv2, consulte los siguientes vínculos:


|       Tipo de contenido       |                                                                                                                                              Referencias                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Recursos de la comunidad**  |                                                                -   [Blog de arquitectura de nube privada](https://blogs.technet.com/b/privatecloud)<br />-Formule preguntas:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   -   [RFC borrador de NVGRE](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **Tecnologías relacionadas** | -Para obtener detalles técnicos de virtualización de red de Hyper-V en Windows Server 2012 R2, consulte [detalles técnicos de la virtualización de red de Hyper-v](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Controladora de red](../../../sdn/technologies/network-controller/Network-Controller.md) |

