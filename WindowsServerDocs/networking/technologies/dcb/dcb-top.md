---
title: Data Center Bridging (DCB)
description: Puede usar este tema para obtener información introductoria acerca del Protocolo de puente del centro de datos en Windows Server 2016.
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: eab47e24ad8c2bd5436b3516babee9237c3c80f3
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865574"
---
# <a name="data-center-bridging-dcb"></a>Protocolo de puente del centro de datos \(DCB\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información introductoria acerca del Protocolo de puente del centro de datos \( DCB \) .

DCB es un conjunto de normas IEEE de Institute of Electrical and Electronics Engineers \( \) que permiten tejidos convergentes en el centro de datos, donde el almacenamiento, las redes de datos, \- el IPC de comunicación entre procesos \( \) y el tráfico de administración comparten la misma infraestructura de red Ethernet.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de DCB
>
>- [Instalar DCB en Windows Server 2016 o Windows 10](dcb-install.md)
>- [Administrar el protocolo de puente del centro de datos (DCB)](dcb-manage.md)

DCB proporciona \- asignación de ancho de banda basada en hardware a un tipo específico de tráfico de red y mejora la confiabilidad del transporte Ethernet con el uso del \- control de flujo basado en la prioridad.

\-La asignación de ancho de banda basada en hardware es fundamental si el tráfico omite el sistema operativo y se descarga en un adaptador de red convergente, lo que podría admitir la interfaz de sistema de equipos pequeños de Internet \( iSCSI \) , el acceso directo a memoria remota \( RDMA \) a través de Ethernet o canal de fibra sobre Ethernet \( FCoE \) .

\-El control de flujo basado en la prioridad es fundamental si el protocolo de nivel superior, como canal de fibra, supone un transporte subyacente sin pérdida.

## <a name="dcb-protocols-and-management-options"></a>Opciones de administración y protocolos de DCB

DCB consta del siguiente conjunto de protocolos.

- ETS de servicio de transmisión mejorada \( \) : IEEE 802.1 QAZ, que se basa en los estándares 802.1 p y 802.1 q
- \(PFC \) de control de flujo de prioridad, IEEE 802.1 QBB
- \(El protocolo de intercambio de DCB DCBX \) , IEEE 802.1 AB, ampliado en el estándar 802.1 QAZ.

El protocolo DCBX le permite configurar DCB en un conmutador, que a continuación puede configurar automáticamente un dispositivo final, como un equipo que ejecuta Windows Server 2016.

Si prefiere administrar DCB desde un conmutador, no es necesario instalar DCB como una característica de Windows Server 2016, pero este enfoque incluye algunas limitaciones.

No obstante, dado que DCBX solo puede informar al adaptador de red del host de tamaños de clase de ETS y habilitación de PFC, los hosts de Windows Server normalmente requieren que DCB esté instalado para que el tráfico se asigne a clases de ETS.

Las aplicaciones de Windows normalmente no están diseñadas para participar en intercambios de DCBX. Por este motivo, el host debe configurarse de forma independiente de los conmutadores de red, pero con la misma configuración.

Si decide administrar DCB desde un conmutador, todavía puede ver las configuraciones en Windows Server 2016 mediante los comandos de Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funcionalidad importante de DCB

A continuación se muestra una lista que resume las funciones proporcionadas por DCB.

1. Proporciona interoperabilidad entre \- adaptadores de red compatibles con DCB y \- conmutadores compatibles con DCB.

2. Proporciona un transporte Ethernet sin pérdida entre un equipo que ejecuta Windows Server 2016 y su conmutador vecino al activar \- el control de flujo basado en la prioridad en el adaptador de red.

3. Proporciona la capacidad de asignar ancho de banda a un control de tráfico \( TC \) por porcentaje, donde el TC puede constar de una o más clases de tráfico que se diferencian en los indicadores de prioridad de las clases de tráfico de 802.1 p \( \) .

4. Permite a los administradores del servidor o a los administradores de red asignar una aplicación a una clase de tráfico o prioridad en particular, en función de protocolos conocidos, el puerto TCP/UDP conocido o el puerto NetworkDirect que use la aplicación.

5. Proporciona administración de DCB a través de Windows Server 2016 Instrumental de administración de Windows \( WMI \) y Windows PowerShell. Para obtener más información, vea la sección [comandos de Windows PowerShell para DCB](#bkmk_wps) más adelante en este tema, además de los temas siguientes.
    - [Componentes de DCB proporcionados por el sistema](/windows-hardware/drivers/network/system-provided-dcb-components)
    - [Requisitos de QoS de NDIS para el protocolo de puente del centro de datos](/windows-hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Proporciona administración de DCB a través de Windows Server 2016 directiva de grupo.

7. Admite la coexistencia de soluciones QoS de calidad de servicio de Windows Server 2016 \( \) .

>[!NOTE]
>Antes de usar cualquier RDMA en la versión RoCE Ethernet convergente \( \) de RDMA, debe habilitar DCB. Aunque no es necesario para redes iWARP de protocolo RDMA de área extensa de Internet \( \) , las pruebas han determinado que todas las \- tecnologías RDMA basadas en Ethernet funcionan mejor con DCB. Por ello, debe considerar la posibilidad de usar DCB para las implementaciones de RDMA de iWARP. Para más información, consulte [Acceso directo a memoria remota (RDMA) y Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Aplicaciones prácticas de DCB

Muchas organizaciones tienen grandes \( \) \( instalaciones San de red de área de almacenamiento FC \) de canal de fibra para el servicio de almacenamiento. La SAN de FC requiere adaptadores de red especiales en los servidores y conmutadores de FC en la red. Estas organizaciones normalmente también usan adaptadores y conmutadores de red Ethernet.

En la mayoría de los casos, el hardware de FC es mucho más costoso de implementar que el hardware Ethernet, lo que da lugar a grandes gastos de capital. Además, el requisito de hardware de conmutador y adaptador de red SAN Ethernet y FC independiente requiere más espacio, energía y capacidad de enfriamiento en un centro de recursos, lo que da lugar a gastos operativos adicionales y continuos.

Desde una perspectiva de costos, resulta ventajoso para muchas organizaciones combinar su tecnología de FC con la \- solución de hardware basada en Ethernet para proporcionar servicios de red de almacenamiento y de datos.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Uso de DCB para un \- tejido convergente basado en Ethernet para las redes de datos y almacenamiento

En el caso de las organizaciones que ya tienen una SAN de FC de gran tamaño pero que desean migrar de una inversión adicional en la tecnología de FC, DCB le permite crear un tejido convergente basado en Ethernet para las redes de datos y almacenamiento. Un tejido convergente de DCB puede reducir el costo total de propiedad en el futuro \( \) y simplificar la administración.

En el caso de los proveedores de hosting que ya han adoptado o que tienen previsto adoptar iSCSI como su solución de almacenamiento, DCB puede proporcionar \- reserva de ancho de banda asistida por hardware para el tráfico iSCSI con el fin de garantizar el aislamiento del rendimiento. Y, a diferencia de otras soluciones de propiedad, DCB se basa en normas \- y, por lo tanto, es relativamente fácil de implementar y administrar en un entorno de red heterogéneo.

Una \- implementación de DCB basada en Windows Server 2016 alivia muchos de los problemas que pueden producirse cuando las soluciones de tejido convergente son proporcionadas por varios OEM de fabricantes de equipos originales \( \) .

Las implementaciones de soluciones de propiedad proporcionadas por varios OEM pueden no interoperar entre sí, pueden ser difíciles de administrar y, por lo general, tendrán distintas programaciones de mantenimiento de software.

Por el contrario, el DCB de Windows Server 2016 se basa en estándares \- y puede implementar y administrar DCB en una red heterogénea.

## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>Comandos de Windows PowerShell para DCB

Hay comandos de Windows PowerShell de DCB para Windows Server 2016 y Windows Server 2012 R2. Puede usar todos los comandos para Windows Server 2012 R2 en Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 comandos de Windows PowerShell para DCB

En el siguiente tema de Windows Server 2016 se proporcionan las descripciones y la sintaxis de los cmdlets de Windows PowerShell para todos los \( \) \( \) cmdlets específicos de QoS Quality of Service de calidad de servicio \- . Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [Módulo DcbQoS](/powershell/module/dcbqos/)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 comandos de Windows PowerShell para DCB

En el siguiente tema de Windows Server 2012 R2 se proporcionan las descripciones y la sintaxis de los cmdlets de Windows PowerShell para todos los \( \) \( \) cmdlets específicos de QoS Quality of Service de calidad de servicio \- . Enumera los cmdlets por orden alfabético según el verbo que aparece al principio del cmdlet.

- [Cmdlets de Calidad de servicio (QoS) del Protocolo de puente del centro de datos (DCB) en Windows PowerShell](/powershell/module/dcbqos/)
