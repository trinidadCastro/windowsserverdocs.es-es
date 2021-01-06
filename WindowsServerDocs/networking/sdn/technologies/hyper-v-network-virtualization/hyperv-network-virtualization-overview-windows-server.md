---
title: Información general de virtualización de red de Hyper-V en Windows Server 2016
description: En este tema se proporciona información general sobre la virtualización de red de Hyper-V en Windows Server 2016
manager: grcusanz
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: 7e4483218c35a99f898701fbdd7fdbe86b021206
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949101"
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Información general de virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016 y Virtual Machine Manager, Microsoft proporciona una solución de virtualización de red de un extremo a otro.  Existen cinco componentes principales que componen la solución de virtualización de red de Microsoft:

-   **Windows Azure Pack para Windows Server** proporciona un portal para inquilinos dirigido a la creación de redes virtuales y un portal administrativo para administrar redes virtuales.

-   **Virtual Machine Manager** (VMM) proporciona una administración centralizada del tejido de red.

-   La **controladora de red de Microsoft** proporciona un punto de automatización programable y centralizado para administrar, configurar, supervisar y solucionar problemas de la infraestructura de red virtual y física en su centro de recursos.

-   **Virtualización de red de Hyper-V** proporciona la infraestructura necesaria para virtualizar el tráfico de red.

-   Las **puertas de enlace de virtualización de red de Hyper-V** proporcionan conexiones entre redes virtuales y físicas.

En este tema se presentan los conceptos y se explican los beneficios y las capacidades clave de la virtualización de red de Hyper-V (una parte de la solución de virtualización de red general) en Windows Server 2016. Se explica de qué manera la virtualización de red beneficia tanto a las nubes privadas dirigidas a la consolidación de la carga de trabajo de la empresa como a los proveedores de servicios de nube pública de infraestructura como Servicio (IaaS).

Para obtener más información técnica sobre la virtualización de redes en Windows Server 2016, consulte [detalles técnicos de la virtualización de red de Hyper-V en Windows server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).

**Lo quiso decir**

-   [Información general de virtualización de red de Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)

-   [Introducción a Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)

-   [Introducción al conmutador virtual de Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
Virtualización de red de Hyper-V ofrece "redes virtuales" (denominadas "redes de VM") a máquinas virtuales de forma similar a la que la virtualización de servidor (hipervisor) proporciona "máquinas virtuales" al sistema operativo. La virtualización de red desacopla las redes virtuales de la infraestructura de red física y quita las restricciones de la asignación de VLAN y dirección IP jerárquica del aprovisionamiento de máquinas virtuales. Esta flexibilidad permite a los clientes pasar fácilmente a las nubes IaaS y permite a los proveedores de servicios de hosting y a administradores de centros de datos administrar sus infraestructuras y, al mismo tiempo, mantener el aislamiento multiempresa necesario, los requisitos de seguridad y la compatibilidad con direcciones IP de máquinas virtuales superpuestas.

Los clientes desean extender a la perfección sus centros de datos a la nube. Hoy existen muchos desafíos técnicos en la creación de dichas arquitecturas de nube híbrida perfectas. Uno de los principales obstáculos a los que se encuentran los clientes es la reutilización de sus topologías de red existentes (subredes, direcciones IP, servicios de red, etc.) en la nube y puentes entre sus recursos locales y sus recursos en la nube.  Virtualización de red de Hyper-V ofrece el concepto de una red de VM que es independiente de la red física subyacente. Con este concepto de red de VM, que se compone de una o más subredes virtuales, la ubicación exacta en la red física de máquinas virtuales conectadas a una red virtual está desacoplada de la topología de red virtual. Como resultado, los clientes pueden mover fácilmente sus subredes virtuales a la nube, y al mismo tiempo, preservar las direcciones IP y la topología existentes en la nube de manera que los servicios existentes continúen funcionando sin tener en cuenta la ubicación física de las subredes. Es decir, Virtualización de red de Hyper-V hace posible una nube híbrida perfecta.

Además de la nube híbrida, muchas organizaciones consolidan sus centros de datos y crean nubes privadas para obtener internamente el beneficio de eficiencia y escalabilidad de las arquitecturas de nube. La virtualización de red de Hyper-V permite una mayor flexibilidad y eficacia para las nubes privadas al desacoplar la topología de red de una unidad de negocio (haciéndola virtual) de la topología de red física real. De esta manera, las unidades de negocio pueden compartir fácilmente una nube privada interna y, al mismo tiempo, estar aisladas entre sí y seguir manteniendo las topologías de red existentes. El equipo de operaciones del centro de datos cuenta con la flexibilidad necesaria para implementar y mover dinámicamente cargas de trabajo en cualquier parte del centro de datos sin interrupciones del servidor, al proporcionar mejores eficiencias operativas y un centro de datos general más efectivo.

Para los propietarios de cargas de trabajo, la ventaja clave es que ahora pueden trasladar sus "topologías" de carga de trabajo a la nube sin cambiar las direcciones IP ni volver a escribir las aplicaciones. Por ejemplo, la aplicación LOB típica de tres niveles se compone de un nivel cliente, un nivel de lógica comercial y un nivel de base de datos. A través de la directiva, Virtualización de red de Hyper-V admite clientes que incorporan todos los niveles de la nube, o partes de los tres niveles de la nube, y al mismo tiempo mantiene la topología de enrutamiento y las direcciones IP de los servicios (es decir, direcciones IP de máquinas virtuales) sin necesidad de cambiar las aplicaciones.

Para los propietarios de infraestructura, la flexibilidad adicional en la ubicación de la máquina virtual permite mover cargas de trabajo en cualquier parte en los centros de datos sin cambiar las máquinas virtuales ni reconfigurar las redes. Por ejemplo, Virtualización de red de Hyper-V permite la migración en vivo entre subredes para que una máquina virtual pueda migrar a cualquier parte del centro de datos sin interrupciones en el servicio. Anteriormente, la migración en vivo se limitaba a la restricción de la misma subred donde se podían ubicar las máquinas virtuales. La migración en vivo entre subredes permite a los administradores consolidar cargas de trabajo en base a requisitos de recursos dinámicos y eficiencia energética, y también albergar el mantenimiento de la infraestructura sin interrumpir el tiempo de funcionamiento de la carga de trabajo del cliente.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
Con el éxito de los centros de datos virtualizados, las organizaciones de TI y los proveedores de hospedaje (proveedores que ofrecen colocación o alquileres de servidores físicos) comenzaron a ofrecer infraestructuras virtualizadas más flexibles que permiten ofrecer a sus clientes instancias de servidor a petición. A esta nueva clase de servicio se la denomina infraestructura como Servicio (IaaS). Windows Server 2016 proporciona todas las funcionalidades de plataforma necesarias para permitir que los clientes empresariales creen nubes privadas y cambien a una TI como modelo operativo de servicio. Windows Server 2016 2016 también permite a los proveedores de hospedaje crear nubes públicas y ofrecer soluciones IaaS a sus clientes. Cuando se combina con Virtual Machine Manager y Windows Azure Pack para administrar la Directiva de virtualización de red de Hyper-V, Microsoft proporciona una potente solución en la nube.

Virtualización de red de Hyper-V de Windows Server 2016 proporciona virtualización de red controlada por software, basada en directivas que reduce la sobrecarga de administración que enfrentan las empresas al expandir las nubes IaaS dedicadas y proporciona mayor flexibilidad y escalabilidad a los proveedores de servicios en la nube para administrar máquinas virtuales con el fin de lograr una mayor utilización de recursos.

Un escenario de IaaS que cuenta con máquinas virtuales de diferentes divisiones organizativas (nube dedicada) o diferentes clientes (nube hospedada) requiere aislamiento seguro. La solución de hoy en día, las redes de área local virtuales (VLAN), pueden presentar desventajas significativas en este escenario.

**VLAN**

Actualmente, las VLAN son el mecanismo que utilizan la mayoría de las organizaciones para admitir la reutilización del espacio de direcciones y el aislamiento de inquilinos. Una VLAN usa etiquetas explícitas (ID de VLAN) en los encabezados de trama Ethernet y depende de los conmutadores Ethernet para aplicar el aislamiento y restringir los nodos de red con el mismo ID de VLAN. Las principales desventajas con VLAN son las siguientes:

-   Mayor riesgo de una interrupción inadvertida debido a la tediosa reconfiguración de conmutadores de producción siempre que las máquinas virtuales o los límites de aislamiento se muevan en el centro de datos dinámico.

-   Escalabilidad limitada ya que existe un número máximo de 4094 VLAN y los conmutadores tradicionales no admiten más de 1000 identificadores de VLAN.

-   Restringido dentro de una sola subred IP, que limita la cantidad de nodos dentro de una sola VLAN y restringe la colocación de maquinas virtuales en base a ubicaciones físicas. Si bien las VLAN se pueden expandir entre los sitios, toda la VLAN debe estar en la misma subred.

**Asignación de dirección IP**

Además de las desventajas que presentan las VLAN, la asignación de direcciones IP de máquinas virtuales presenta problemas, entre los que se incluyen:

-   Las ubicaciones físicas en la infraestructura de red del centro de datos determinan las direcciones IP de las máquinas virtuales. Como resultado, pasar a la nube generalmente implica cambiar las direcciones IP de las cargas de trabajo de servicio.

-   Las directivas están unidas a direcciones IP, como las reglas de firewall, los servicios de directorio y detección de recursos, etc. Cambiar las direcciones IP requiere actualizar todas las directivas asociadas.

-   La implementación de máquinas virtuales y el aislamiento del tráfico dependen de la topología.

Cuando los administradores de red de centro de datos planifican el diseño físico del centro de datos, debe tomar decisiones al respecto en los casos en los que las subredes se colocarán y se enrutarán físicamente. Estas decisiones se basan en la tecnología IP y Ethernet que influyen en las posibles direcciones IP admitidas para máquinas virtuales que se ejecutan en un servidor o blade determinado conectado a un bastidor en particular del centro de datos. Cuando se proporciona y se coloca una máquina virtual en el centro de datos, la misma debe cumplir con estas elecciones y restricciones en lo que a la dirección IP respecta. Por lo tanto, el resultado típico es que los administradores del centro de datos asignen nuevas direcciones IP a las máquinas virtuales.

El problema con este requisito es que además de ser una dirección, existe información semántica asociada con una dirección IP. Por ejemplo, es posible que una subred contenga determinados servicios o esté en una ubicación física distinta. Las reglas de firewall, las directivas de control de acceso, las asociaciones de seguridad de IPsec están comúnmente asociadas con direcciones IP. Cambiar las direcciones IP obliga a los propietarios de máquinas virtuales a ajustar todas las directivas que se basaban en la dirección IP original. Esta sobrecarga de cambio de numeración es tan alta que muchas empresas optan por implementar únicamente servicios nuevos en la nube, dejando de lado las aplicaciones heredadas.

Virtualización de red de Hyper-V desacopla las redes virtuales para las máquinas virtuales del cliente de la infraestructura de red física. Como resultado, permite a las máquinas virtuales de clientes mantener las direcciones IP originales, y al mismo tiempo, permite a los administradores de centros de datos proporcionar máquinas virtuales de clientes en cualquier parte del centro de datos sin reconfigurar direcciones IP físicas o ID de VLAN. La siguiente sección resume la funcionalidad clave.

## <a name="important-functionality"></a><a name="BKMK_NEW"></a>Funcionalidad importante
A continuación se muestra una lista de la funcionalidad, las ventajas y las capacidades clave de la virtualización de red de Hyper-V en Windows Server 2016:

-   **Habilita la colocación de cargas de trabajo flexible: aislamiento de red y reutilización de direcciones IP sin VLAN**

    Virtualización de red de Hyper-V desacopla las redes virtuales del cliente de la infraestructura de red física de los anfitriones, lo que proporciona libertad para las ubicaciones de carga de trabajo dentro de los centros de recursos. La colocación de carga de trabajo de máquina virtual ya no se limita a la asignación de direcciones IP o a requisitos de aislamiento VLAN de las redes físicas, ya que se aplica dentro de los hosts Hyper-V en base a directivas de virtualización multiempresa definidas por el software.

    Máquinas virtuales de diferentes clientes con direcciones IP superpuestas ahora pueden implementarse en el mismo servidor host sin necesidad de recurrir a tediosas configuraciones de VLAN y sin tener que violar la jerarquía de direcciones IP. Esto puede optimizar la migración de las cargas de trabajo del cliente en proveedores de hospedaje IaaS compartido, lo que permite a los clientes mover estas cargas de trabajo sin modificación, que incluye dejar intactas las direcciones IP de las máquinas virtuales. Para el proveedor de hospedaje, admitir varios clientes que desean extender el espacio de dirección de red existente al centro de datos IaaS compartido es un ejercicio complejo que implica configurar y mantener VLAN aisladas para que cada cliente garantice la coexistencia de espacios de direcciones posiblemente superpuestas. Con Virtualización de red de Hyper-V, admitir direcciones superpuestas es más fácil y requiere menos reconfiguración de red por parte del proveedor de hospedaje.

    Además, el mantenimiento y las actualizaciones de infraestructura física se pueden realizar sin causar tiempo de inactividad de las cargas de trabajo del cliente. Con Virtualización de red de Hyper-V, las máquinas virtuales en un host, bastidor, subred, VLAN o clúster completo específico pueden migrar sin necesidad de realizar un cambio de dirección IP física o una reconfiguración importante.

-   **Permite movimientos más fáciles de las cargas de trabajo a una nube IaaS compartida**

    Con Virtualización de red de Hyper-V, las direcciones IP y las configuraciones de máquinas virtuales se mantienen intactas. Esto permite a las organizaciones de TI mover las cargas de trabajo con mayor facilidad desde los centros de datos a un proveedor de hospedaje de IaaS compartido con mínima reconfiguración de la carga de trabajo o de las herramientas y directivas de infraestructura. En los casos en los que existe conectividad entre los dos centros de datos, los administradores de TI pueden continuar usando sus herramientas sin reconfigurarlas.

-   **Permite la migración en vivo entre subredes**

    La migración en vivo de las cargas de trabajo de máquinas virtuales tradicionalmente se ha limitado a la misma subred IP o VLAN porque el cruce de subredes requería que el sistema operativo invitado de la máquina virtual cambiara su dirección IP. Este cambio de dirección quiebra la comunicación existente e interrumpe los servicios que se ejecutan en la máquina virtual. Con virtualización de red de Hyper-V, las cargas de trabajo se pueden migrar en vivo desde servidores que ejecutan Windows Server 2016 en una subred a servidores que ejecutan Windows Server 2016 en una subred diferente sin cambiar las direcciones IP de la carga de trabajo. Virtualización de red de Hyper-V garantiza que cambie la ubicación de la máquina virtual debido a que se actualiza y se sincroniza la migración en vivo entre los hosts que tienen comunicaciones continuas con la máquina virtual migrada.

-   **Permite una administración más fácil del servidor desacoplado y la administración de la red**

    La colocación de la carga de trabajo del servidor se simplifica ya que la migración y la colocación de las cargas de trabajo son independientes de las configuraciones de redes físicas subyacentes. Los administradores de los servidores pueden centrarse en la administración de servicios y de servidores y los administradores de servidores pueden centrarse en la administración general de la infraestructura y el tráfico de red. Esto permite a los administradores de servidores de centros de datos implementar y migrar máquinas virtuales sin cambiar las direcciones IP de las máquinas virtuales. La sobrecarga se reduce ya que Virtualización de red de Hyper-V permite que se produzca la colocación de máquinas virtuales independientemente de la topología de red, reduciendo la necesidad de que los administradores de red participen en colocaciones que podrían cambiar los límites de aislamiento.

-   **Simplifica la red y mejora la utilización de recursos del servidor o de la red**

    La rigidez de las VLAN y la dependencia de la colocación de máquinas virtuales en una infraestructura de red física genera sobreaprovisionamiento e infrautilización. Al romper con la dependencia, el aumento de flexibilidad de colocación de carga de trabajo de máquinas virtuales puede simplificar la administración de red y mejorar la utilización de recursos de red y de servidor. Tenga en cuenta que Virtualización de red de Hyper-V admite VLAN en el contexto del centro de datos físico. Por ejemplo, es posible que un centro de datos desee que todo el tráfico de Virtualización de red de Hyper-V se encuentre en una VLAN específica.

-   **Es compatible con la infraestructura existente y la tecnología emergente**

    La virtualización de red de Hyper-V se puede implementar en el centro de recursos de hoy en día, pero es compatible con las tecnologías de "red sin formato" del centro de recursos emergente.

    Por ejemplo, HNV en Windows Server 2016 admite el formato de encapsulación VXLAN y el protocolo de administración de bases de datos (OVSDB) de Open vSwitch como la interfaz SouthBound (SBI).

-   **Proporciona interoperabilidad y disponibilidad del ecosistema**

    Virtualización de red de Hyper-V admite varias configuraciones para la comunicación con recursos existentes, como la conectividad entre instalaciones, la red de área de almacenamiento (SAN), el acceso a recursos no virtualizados, etc. Microsoft se compromete a trabajar con socios del ecosistema para dar soporte técnico y mejorar la experiencia de Virtualización de red de Hyper-V en términos de rendimiento, escalabilidad y funcionalidad de administración.

-   **Configuración basada en directivas**

    Las directivas de virtualización de red en Windows Server 2016 se configuran a través de la controladora de red de Microsoft. La controladora de red tiene una API de Northbound RESTful y una interfaz de Windows PowerShell para configurar la Directiva. Para obtener más información acerca de la controladora de red de Microsoft, consulte [controladora de red](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
La virtualización de red de Hyper-V que usa la controladora de red de Microsoft requiere Windows Server 2016 y el rol Hyper-V.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Otras referencias
Para obtener más información acerca de la virtualización de red de Hyper-V en Windows Server 2016, consulte los siguientes vínculos:


|       Tipo de contenido       |                                                                                                                                Referencias                                                                                                                                |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Recursos de la comunidad**  |     -   [Blog de arquitectura de nube privada](/archive/blogs/privatecloud/cloud-datacenter-network-architecture-in-the-windows-server-8-era)<br />-Formule preguntas: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)     |
|         **RFC**          |                                                                                                     -VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                                                      |
| **Tecnologías relacionadas** | -   [Controladora de red](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Información general de virtualización de red de Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2) |