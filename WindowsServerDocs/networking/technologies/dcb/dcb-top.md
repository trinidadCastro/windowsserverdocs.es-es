---
title: Protocolo de puente del centro de datos (DCB)
description: Puede usar este tema para obtener información introductoria acerca de puente del centro de datos en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb488192a873743db27d9c1d09c5912bc810bb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834186"
---
# <a name="data-center-bridging-dcb"></a>Puente de centros de datos \(DCB\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información introductoria sobre el protocolo de puente del centro de datos \(DCB\).

DCB es un conjunto de Institute of Electrical and Electronics Engineers \(IEEE\) estándares que permiten usar tejidos convergentes en el centro de datos, donde el almacenamiento, redes, de datos clúster Inter\-proceso comunicación \(IPC\), y todo el tráfico de administración comparten la misma infraestructura de red Ethernet.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de DCB
>
>- [Instalar DCB en Windows Server 2016 o Windows 10](dcb-install.md)
>- [Administrar el centro de datos (DCB) de protocolo de puente](dcb-manage.md)

DCB proporciona hardware\-en función de asignación de ancho de banda a un tipo específico de tráfico de red y mejora la confiabilidad del transporte Ethernet mediante el uso de prioridad\-en función de control de flujo.

Hardware\-asignación en función del ancho de banda es fundamental si el tráfico omite el sistema operativo y se descarga a un adaptador de red convergente, que puede admitir Internet Small Computer System Interface \(iSCSI\), Acceso a memoria directa remota \(RDMA\) sobre Ethernet o canal de fibra sobre Ethernet \(FCoE\).

Prioridad\-control de flujo basado en es fundamental si el protocolo de nivel superior, como canal de fibra, supone un transporte subyacente sin pérdida de datos.

## <a name="dcb-protocols-and-management-options"></a>DCB protocolos y las opciones de administración

DCB está formado por el siguiente conjunto de protocolos. 

- Mejorado servicio de transmisión \(ETS\) – IEEE 802.1Qaz, que se basa en el 802.1p y 802.1Q estándares
- Control de flujo de prioridad \(PFS\), IEEE 802.1Qbb 
- Protocolo de intercambio de DCB \(DCBX\), IEEE 802.1AB, extendido en el estándar 802.1Qaz.

El protocolo DCBX le permite configurar DCB en un conmutador, que, a continuación, puede configurar automáticamente un dispositivo final, como un equipo que ejecuta Windows Server 2016.

Si prefiere administrar DCB de un conmutador, no es necesario instalar DCB como una característica de Windows Server 2016, sin embargo, este enfoque incluye algunas limitaciones.

Porque DCBX solo puede informar al adaptador de red de host de tamaños de la clase ETS y habilitación de PFC, sin embargo, los hosts de Windows Server suelen requieran que se instala DCB para que el tráfico se asigna a las clases ETS.

Normalmente, las aplicaciones de Windows no están diseñadas para participar en intercambios DCBX. Por este motivo, el host debe configurarse por separado de los conmutadores de red, pero con los mismos valores.

Si decide administrar DCB de un conmutador, todavía puede ver las configuraciones en Windows Server 2016 mediante el uso de comandos de Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funcionalidad importante de DCB

A continuación se muestra una lista que resume las funciones proporcionadas por DCB.

1. Proporciona interoperabilidad entre DCB\-adaptadores de red con capacidad y DCB\-conmutadores compatibles con.

2. Proporciona un transporte Ethernet sin pérdida de datos entre un equipo que ejecuta Windows Server 2016 y su conmutador más cercano al activar prioridad\-en función de control de flujo en el adaptador de red.

3. Proporciona la capacidad de asignar ancho de banda a un Control de tráfico \(TC\) por porcentaje, donde el TC puede consistir en una o más clases de tráfico que se diferencian por la clase de tráfico de 802.1p \(prioridad\) indicadores.

4. Permite a los administradores del servidor o a los administradores de red asignar una aplicación a una clase de tráfico o prioridad en particular, en función de protocolos conocidos, el puerto TCP/UDP conocido o el puerto NetworkDirect que use la aplicación.

5. Proporciona administración de DCB a través de Instrumental de administración de Windows de Windows Server 2016 \(WMI\) y Windows PowerShell. Para obtener más información, consulte la sección [comandos de Windows PowerShell para DCB](#bkmk_wps) más adelante en este tema, además de los temas siguientes.
    - [Componentes proporcionados por el sistema DCB](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Requisitos de QoS NDIS de puente del centro de datos](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Proporciona administración de DCB a través de la directiva de grupo de Windows Server 2016.

7. Admite la coexistencia de calidad de servicio de Windows Server 2016 \(QoS\) soluciones.

>[!NOTE]
>Antes de usar cualquier RDMA sobre Ethernet convergente \(RoCE\) versión de RDMA, debe habilitar DCB. Aunque no es necesario para el protocolo de Internet amplia área RDMA \(iWARP\) redes, las pruebas ha determinado que Ethernet todas\-en función de trabajo de las tecnologías RDMA mejor con DCB. Por este motivo, considere la posibilidad de usar DCB para las implementaciones de RDMA de iWARP. Para obtener más información, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Aplicaciones prácticas de DCB

Muchas organizaciones tienen grandes de canal de fibra \(FC\) red de área de almacenamiento \(SAN\) instalaciones para servicio de almacenamiento. La SAN de FC requiere adaptadores de red especiales en los servidores y conmutadores de FC en la red. Estas organizaciones también usan generalmente conmutadores y adaptadores de red Ethernet.

En la mayoría de los casos, el hardware de FC es mucho más costoso de implementar que el hardware de Ethernet, lo cual genera grandes gastos de capital. Además, el requisito de Ethernet y SAN de FC red adaptador y el conmutador de hardware independiente requiere espacio adicional, alimentación y refrigeración de capacidad en un centro de datos, lo que resulta en gastos operativos constantes adicionales.

Desde una perspectiva de costos es una ventaja para muchas organizaciones combinar su tecnología de FC con su Ethernet\-en función de la solución de hardware para ofrecer almacenamiento y datos de servicios de red.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Usar DCB para Ethernet\-tejido convergido para redes de datos y almacenamiento basado en

Para las organizaciones que ya tienen una SAN de FC de gran tamaño, pero desean migrar sin inversión adicional en la tecnología FC, DCB puede generar una basada en Ethernet convergente fabric para el almacenamiento y las redes de datos. Un DCB convergente tejido puede reducir el costo total futuras de la propiedad \(TCO\) y simplificar la administración.

Los proveedores de hospedaje que ya han adoptado o que planifican adoptar iSCSI como su solución de almacenamiento, DCB puede proporcionar hardware\-asistida de reserva de ancho de banda para el tráfico de iSCSI garantizar el aislamiento del rendimiento. Y a diferencia de otras soluciones de propietario, DCB es estándares\-basada - y, por tanto, es relativamente fácil de implementar y administrar en un entorno de red heterogénea.

Windows Server 2016\-alivia la implementación en función de DCB muchos de los problemas que pueden producirse cuando varios fabricantes de equipos originales proporciona soluciones de fabric \(OEM\).

Las implementaciones de soluciones de propietario proporcionadas por varios OEM pueden no interoperar entre sí, puede ser difíciles de administrar y, normalmente tendrá las programaciones de mantenimiento de software diferente. 

En cambio, Windows Server 2016 DCB es estándares\-en función, y puede implementar y administrar DCB en una red heterogénea.

## <a name="bkmk_wps"></a>Comandos de Windows PowerShell para DCB

Hay comandos DCB Windows PowerShell para Windows Server 2016 y Windows Server 2012 R2. Puede usar todos los comandos de Windows Server 2012 R2 en Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos de Windows Server 2016 Windows PowerShell para DCB

El siguiente tema para Windows Server 2016 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los datos de puente del centro de \(DCB\) calidad de servicio \(QoS\)\-cmdlets específicos. Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows PowerShell comandos de Windows Server 2012 R2 para DCB

El siguiente tema para Windows Server 2012 R2 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los datos de puente del centro de \(DCB\) calidad de servicio \(QoS\)\-cmdlets específicos. Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [Centro de datos (DCB) Cmdlets de calidad de servicio (QoS) en Windows PowerShell de protocolo de puente](https://technet.microsoft.com/library/hh967440.aspx)
