---
title: Información general de la virtualización de red de Hyper-V en Windows Server 2016
description: En este tema proporciona una descripción general de la virtualización de la red de Hyper-V en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c20f9b2d81ac2d49ed0bbea5f3aca48dea0c50
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Información general de la virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En Windows Server 2016 y Virtual Machine Manager, Microsoft proporciona una solución de virtualización de la red de principio a fin.  Hay cinco componentes principales que componen la solución de virtualización de red de Microsoft:  
  
-   **Windows Azure Pack para Windows Server** proporciona un inquilino de cara al portal para crear redes virtuales y un portal de administración para administrar las redes virtuales.  
  
-   **Virtual Machine Manager** (VMM) proporciona una administración centralizada de la estructura de la red.  
  
-   **Controlador de red de Microsoft** proporciona un punto programable centralizado de la automatización para administrar, configurar, supervisar y solucionar problemas de infraestructura de red físicas y virtuales en el centro de datos.  
  
-   **Virtualización de Hyper-V red** proporciona la infraestructura necesaria para virtualizar el tráfico de red.  
  
-   **Puertas de enlace de virtualización de Hyper-V red** proporcionar conexiones entre redes físicas y virtuales.  
  
Este tema presenta los conceptos y explica las ventajas clave y las funcionalidades de virtualización de red de Hyper-V (una parte de la solución de virtualización de red general) en Windows Server 2016. Se explica cómo beneficia virtualización de la red a ambos nubes privadas buscando consolidación de carga de trabajo de la empresa y proveedores de servicios de nube pública de infraestructura como servicio (IaaS).  
  
Para obtener más información sobre la virtualización de red en Windows Server 2016, consulta [detalles técnicos virtualización de Hyper-V red en Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  
  
**Te refieres**  
  
-   [Introducción a la virtualización de Hyper-V red](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  
  
-   [Información general de Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Introducción a conmutador Virtual de Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>Descripción de la característica  
Virtualización de la red de Hyper-V ofrece "redes virtuales" (denominadas en una red de la máquina virtual) en las máquinas virtuales similares a cómo virtualización (hipervisor) ofrece "máquinas virtuales" al sistema operativo. Virtualización de la red desacopla redes virtuales de la infraestructura de red física y quita las restricciones de VLAN y asignación de direcciones IP jerárquica de aprovisionamiento de la máquina virtual. Esta flexibilidad facilita los clientes pueden mover a IaaS nubes y eficaz de anfitriones y administradores del centro de datos administrar su infraestructura, manteniendo el aislamiento de varios inquilino es necesario, los requisitos de seguridad, y direcciones IP de máquina Virtual de superpuestas auxiliares.  
  
Los clientes quieren ampliar sus centros de datos en la nube de sin problemas. Actualmente, existen desafíos técnicos en hacer que estos arquitecturas de nube híbrida sin problemas. Uno de los mayores enfrentan los clientes de los obstáculos es reutilizar sus topologías de red existente (subredes, IP direcciones, servicios de red y así sucesivamente.) en la nube y puente entre sus recursos locales y sus recursos de nube.  Virtualización de Hyper-V red proporciona el concepto de una red de máquina virtual que es independiente de la red física subyacente. Con este concepto de una red de la máquina virtual, compuesto por una o varias subredes Virtual, la ubicación exacta en la red física de máquinas virtuales conectados a una red virtual está desconectada de la topología de red virtual. Como resultado, los clientes pueden mover con facilidad sus subredes virtuales a la nube mientras conservan sus direcciones IP y topología en la nube para que los servicios existentes seguirán funcionando sin reconocimiento de la ubicación física de las subredes. Es decir, virtualización de Hyper-V en red permite una nube híbrida sin problemas.  
  
Además de nube híbrida, muchas organizaciones se consolidando sus centros de datos y se crean nubes privadas para internamente aprovechar las ventajas de eficacia y la escalabilidad de arquitecturas de nube. Virtualización de Hyper-V en red permite mejor flexibilidad y eficacia para topología de red de nubes privadas por desacoplar una unidad de negocio (por lo que virtual) desde la topología de red físicos reales. De este modo, las empresas fácilmente pueden compartir una nube privada interna al ser aislado entre sí y seguir manteniendo las topologías de red existente. El equipo de operaciones de datacenter dispone de flexibilidad para implementar y muevan dinámicamente las cargas de trabajo en cualquier lugar en el centro de datos sin interrupciones del servidor que proporciona la mejor eficacia operativa y un centro de datos más efectiva general.  
  
Para los propietarios de carga de trabajo, la principal ventaja es que puede ahora cambien su carga de trabajo "topologías" en la nube sin cambiar sus direcciones IP o volver a escribir sus aplicaciones. Por ejemplo, la aplicación de LOB tres niveles típica se compone de un nivel de front-end, un nivel de la lógica empresarial y un nivel de base de datos. Mediante la directiva, virtualización de Hyper-V en red permite al cliente incorporación todos los o partes de los tres niveles en la nube, mientras mantienes la topología de enrutamiento y las direcciones IP de los servicios (es decir, máquina virtual direcciones IP), sin necesidad de que las aplicaciones cambiarse.  
  
Para los propietarios de infraestructura, la flexibilidad adicional en la colocación de máquina virtual hace posible mover cargas de trabajo en cualquier lugar en los centros de datos sin cambiar las máquinas virtuales o volver a configurar las redes. Por ejemplo virtualización de Hyper-V en red permite migración en vivo cruzado subred para que una máquina virtual dinámicos puede migrar desde cualquier lugar en el centro de datos sin una interrupción del servicio. Anteriormente la migración en vivo estaba limitada a la misma subred restringir donde podría encontrarse máquinas virtuales. Entre subred migración en vivo permite a los administradores consolidar cargas de trabajo en función de requisitos de recursos dinámicos, la eficiencia energética y también puede acomodar el mantenimiento de la infraestructura sin interrumpir la carga de trabajo de cliente el tiempo.  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Con el éxito de centros de datos virtualizado, las organizaciones de TI y los proveedores de hospedaje (proveedores que ofrecen ubicación cooperativa o alquileres servidor físico) han comenzado a ofrecer más flexibles infraestructuras virtualizadas que hacen que sea más sencillo ofrecer instancias del servidor petición a sus clientes. Esta nueva clase de servicio se conoce como infraestructura como servicio (IaaS). Windows Server 2016 proporciona todas las funcionalidades de plataforma necesarios para permitir que los clientes de empresa compilar la transición a TI como un modelo de funcionamiento del servicio y nubes privadas. Windows Server 2016 2016 también permite anfitriones generar las nubes públicas y ofrecen soluciones de IaaS a sus clientes. Cuando se combina con Virtual Machine Manager y Windows Azure Pack para administrar la directiva de red virtualización de Hyper-V, Microsoft proporciona una solución de nube eficaces.  
  
Virtualización de red de Windows Server 2016 Hyper-V proporciona virtualización de red basada en directivas, controladas por software que reduce la sobrecarga que se enfrentan las empresas cuando estos se amplían dedicados nubes IaaS, y proporciona a anfitriones de nube escalabilidad y una mayor flexibilidad para administrar las máquinas virtuales para lograr una mayor utilización de recursos de administración.  
  
Un escenario de IaaS que tiene máquinas virtuales desde diferentes divisiones organizativas (nube dedicados) o los clientes diferentes (nube hospedado) requiere aislamiento seguro. Solución de hoy en día, redes de área local (VLAN), virtuales puede presentar inconvenientes significativos en este escenario.  
  
**VLAN**  
  
Actualmente, VLAN son el mecanismo que la mayoría de las organizaciones que se usa para permitir la reutilización del espacio de direcciones y aislamiento de inquilinos. Una VLAN usa explícito etiquetado (VLAN ID) en los encabezados de marcos de Ethernet, y se basa en los conmutadores de Ethernet para exigir el aislamiento y restringir el tráfico a los nodos de la red con el mismo identificador de VLAN. Los inconvenientes principales con VLAN son los siguientes:  
  
-   Mayor riesgo de que una interrupción involuntaria debido a una engorrosa reconfiguración de producción cambia cada vez que se mueven de máquinas virtuales o límites de aislamiento del centro de datos dinámicos.  
  
-   Limitado en escalabilidad porque hay un máximo de 4094 VLAN y conmutadores típicas son compatibles con los identificadores de VLAN no más de 1.000.  
  
-   Está restringido dentro de una única subred IP, que limita el número de nodos dentro de una sola VLAN y restringe la colocación de máquinas virtuales en función de las ubicaciones físicas. Aunque pueden ampliarse VLAN en sitios, debe ser la VLAN toda en la misma subred.  
  
**Asignación de direcciones IP**  
  
Además de los inconvenientes que presentan VLAN, la asignación de direcciones IP de máquina virtual presenta problemas, que incluyen:  
  
-   Ubicaciones físicas de infraestructura de red de centros de datos determinan direcciones IP de máquina virtual. Como resultado, es necesario cambiar de direcciones IP de las cargas de trabajo de servicio mover normalmente en la nube.  
  
-   Las directivas están vinculadas a direcciones IP, como las reglas del firewall, recursos detección y servicios de directorio y así sucesivamente. Cambiar direcciones IP, es necesario actualizar todas las directivas asociadas.  
  
-   Implementación de máquinas virtuales y aislamiento de tráfico dependen de la topología.  
  
Cuando los administradores de red de centros de datos plan el diseño físico del centro de datos, deben tomar decisiones sobre dónde subredes se físicamente coloca y se enrutan. Estas decisiones se basan en la tecnología IP y Ethernet que influyen en las direcciones IP posibles permitidos para las máquinas virtuales se ejecutan en un servidor determinado o un módulo que está conectado a un bastidor particular en el centro de datos. Cuando se aprovisiona una máquina virtual y se coloca en el centro de datos, deben cumplir con estas opciones y restricciones en relación con la dirección IP. Por lo tanto, lo normal es que los administradores de los centros de datos asignan nuevas direcciones IP a las máquinas virtuales.  
  
El problema con este requisito es que, además de ser una dirección, hay información semántica asociada con una dirección IP. Por ejemplo, una subred puede contener dada servicios o estar en una ubicación física diferente. Las reglas del firewall, las directivas de control de acceso y asociaciones de seguridad IPsec se asocian habitualmente a direcciones IP. Cambio de direcciones IP obliga a los propietarios de máquina virtual para ajustar sus todas las directivas que se basan en la dirección IP original. Esta nueva numeración sobrecarga es tan alta que muchas empresas elegir implementar únicamente nuevos servicios en la nube, y dejar solamente los aplicaciones heredadas.  
  
Virtualización de Hyper-V red desacopla redes virtuales para máquinas virtuales de cliente de la infraestructura de red física. Como resultado, permite mantener sus direcciones IP originales, permitiendo que los administradores datacenter aprovisionar máquinas virtuales de cliente en cualquier lugar en el centro de datos sin tener que volver a configurar física direcciones IP o VLAN identificadores de máquinas virtuales al cliente. La siguiente sección resume la funcionalidad clave.  
  
## <a name="BKMK_NEW"></a>Funcionalidad importante  
La siguiente es una lista de la funcionalidad clave, beneficios y las funcionalidades de virtualización de la red de Hyper-V en Windows Server 2016:  
  
-   **Permite la colocación de carga de trabajo flexible - IP dirección volver a usar sin VLAN y aislamiento de red**  
  
    Virtualización de Hyper-V red desacopla las redes virtuales del cliente de la infraestructura de red física de los anfitriones, que proporciona la libertad de ubicaciones de carga de trabajo dentro de los centros de datos. Colocación de carga de trabajo de máquina virtual ya no está limitado a la asignación de direcciones IP o los requisitos de aislamiento de VLAN de la red física porque se aplica dentro de hosts de Hyper-V en función de las directivas de virtualización definidos por el software y para varios inquilinos.  
  
    Máquinas virtuales de los distintos clientes con direcciones IP superpuestas ahora pueden implementarse en el mismo servidor host sin requerir engorroso configuración VLAN o infringir la jerarquía de la dirección IP. Esto puede simplificar la migración de las cargas de trabajo de cliente en compartido IaaS hospedaje de proveedores de permitir a los clientes mover las cargas de trabajo sin modificaciones, que incluye dejar las direcciones IP de máquina virtual sin cambios. Para el proveedor de hospedaje, la compatibilidad con muchos de los clientes que quieren ampliar su espacio de direcciones de red existentes en el centro de datos compartida de IaaS es un ejercicio complejo de configuración y mantenimiento VLAN aisladas para cada cliente garantizar la coexistencia de potencialmente superpuestos espacios de direcciones. Con la virtualización de la red de Hyper-V, compatibilidad con direcciones superpuestas es más fácil y requiere menos reconfiguración de red por el proveedor de hospedaje.  
  
    Además, las actualizaciones y mantenimiento de la infraestructura físico pueden hacerse sin provocar un tiempo de inactividad de cargas de trabajo de cliente. Con la virtualización de la red de Hyper-V, máquinas virtuales en un host específico, bastidor, subred, VLAN o en todo el clúster se pueden migrar sin necesidad de un cambio de dirección IP física o reconfiguración principales.  
  
-   **Permite facilitar el desplazamiento de las cargas de trabajo a una nube IaaS compartida**  
  
    Con la virtualización de la red de Hyper-V, direcciones IP y configuraciones de máquina virtual no se modifica. Esto permite mover más fácilmente las cargas de trabajo desde sus centros de datos a un proveedor de hospedaje de IaaS compartido con reconfiguración mínima de la carga de trabajo o sus directivas y las herramientas de infraestructura de las organizaciones de TI. En casos donde hay conectividad entre dos centros de datos, los administradores de TI pueden seguir usando sus herramientas sin volver a configurarlos.  
  
-   **Habilita la migración en vivo en subredes**  
  
    Migración en vivo de cargas de trabajo de máquina virtual tradicionalmente ha limitado a la misma subred IP o VLAN porque cruzar las subredes requisitos de sistema de operativo invitado de la máquina virtual para cambiar su dirección IP. Este cambio de dirección saltos de comunicación existente y deteriore los servicios que se ejecutan en la máquina virtual. Con la virtualización de la red de Hyper-V, las cargas de trabajo pueden ser dinámicos migrar desde servidores que ejecutan Windows Server 2016 en una subred a servidores que ejecutan Windows Server 2016 en una subred diferente sin cambiar las direcciones IP de carga de trabajo. Virtualización de Hyper-V red garantiza que cambios de ubicación de la máquina virtual debido a la migración en vivo se actualiza y se sincronizan entre los hosts que tienen comunicación continua con la máquina virtual migrada.  
  
-   **Permite una administración más sencilla de administración de servidor y red desacoplada**  
  
    Ubicación de carga de trabajo del servidor se simplifica porque migración y la colocación de las cargas de trabajo son independientes de las configuraciones de red física subyacente. Los administradores de servidor puedan concentrarse en administrar servicios y los servidores y los administradores de red pueden centrarse en la administración general de tráfico y la infraestructura de red. Esto permite a los administradores de servidor de centros de datos implementar y máquinas virtuales se migran sin cambiar la dirección IP de las máquinas virtuales. Allí se reduce sobrecarga porque virtualización de Hyper-V en red permite la posición de máquina virtual que se produzcan independientemente de la topología de red, lo que reduce la necesidad de que los administradores de red participar en posiciones que pueden cambiar los límites de aislamiento.  
  
-   **Simplifica la red y mejora la utilización de recursos del servidor o la red**  
  
    Solidez de VLAN y la dependencia de colocación de la máquina virtual en un resultado de la infraestructura de red física en infrautilización y para otros fines. Al cortar la dependencia, la mayor flexibilidad de colocación de carga de trabajo de máquina virtual puede simplificar la administración de red y mejorar servidor y utilización de recursos de red. Ten en cuenta que la virtualización de Hyper-V red admite VLAN en el contexto del centro de datos físico. Por ejemplo, un centro de datos que desee todo el tráfico de red virtualización de Hyper-V a estar en una VLAN específica.  
  
-   **Es compatible con la infraestructura existente y tecnología emergente**  
  
    Virtualización de Hyper-V en red puede implementarse en el centro de datos de hoy, pero es compatible con tecnologías de "red plana" datacenter emergentes.  
  
    Por ejemplo, HNV en Windows Server 2016 admite el formato de encapsulación VXLAN y el protocolo de administración de base de datos (OVSDB) del vSwitch abierto como la interfaz SouthBound (SBI)..  
  
-   **Proporciona la preparación de la interoperabilidad y ecosistema para**  
  
    Virtualización de Hyper-V red admite varias configuraciones para la comunicación con los recursos existentes, como entre conectividad local, red de área de almacenamiento (SAN), acceso a los recursos no virtualizados y así sucesivamente. Microsoft se compromete a trabajar con socios del ecosistema para admitir y mejorar la experiencia de virtualización de la red de Hyper-V en cuanto a rendimiento, la escalabilidad y facilidad de uso.  
  
-   **Configuración basada en directivas**  
  
    Las directivas de la virtualización de red en Windows Server 2016 se configuran mediante el controlador de red de Microsoft. El controlador de red tiene una API de REST northbound y la interfaz de Windows PowerShell para configurar la directiva. Para obtener más información acerca del controlador de red de Microsoft, consulta [controlador de red](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Virtualización de Hyper-V red mediante el controlador de red de Microsoft requiere el rol de Hyper-V y de Windows Server 2016.  
  
## <a name="BKMK_LINKS"></a>Consulta también  
Para obtener más información sobre la virtualización de la red de Hyper-V en Windows Server 2016 consulta los siguientes vínculos:  
  
|Tipo de contenido|Referencias|  
|----------------|--------------|  
|**Recursos de la Comunidad**|-   [Blog de arquitectura de nube privada](http://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Preguntas:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN - [RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologías relacionadas**|-   [Controlador de red](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Introducción a la virtualización de Hyper-V red](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)|  
  


