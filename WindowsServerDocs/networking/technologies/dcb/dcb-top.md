---
title: Puente (DCB) del centro de datos
description: Puedes usar este tema para obtener información básica acerca de puente de centro de datos en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d465e855adc387d7136919ac11fbab56c792c34
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="data-center-bridging-dcb"></a>Puente \(DCB\) del centro de datos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información de introducción sobre \(DCB\) puente de centro de datos.

DCB es un conjunto de estándares \(IEEE\) Instituto de ingenieros eléctricos y electrónicos que permiten a los tejidos convergido en el centro de datos, donde almacenamiento, funciones de red de datos, clúster \(IPC\) comunicación Inter\ procesos y tráfico de administración comparten la misma infraestructura de red Ethernet.

>[!NOTE]
>Además de este tema, la siguiente documentación DCB está disponible
>
>- [Instalar DCB en Windows Server 2016 o Windows 10](dcb-install.md)
>- [Administrar el centro de datos puente (DCB)](dcb-manage.md)

DCB proporciona la asignación de ancho de banda según hardware\ a un tipo específico de tráfico de red y mejora la fiabilidad de transporte de Ethernet con el uso del control de flujo de la priority\.

Asignación de ancho de banda basada en Hardware\ es fundamental si tráfico omite el sistema operativo y se descarga a un adaptador de red convergente, que es posible que admitan \(iSCSI\) interfaz del sistema de equipos pequeños de Internet, acceso directo de memoria remoto \(RDMA\) sobre Ethernet o Fibre Channel sobre Ethernet \(FCoE\).

Control de flujo en función de Priority\ es fundamental si el protocolo de nivel superior, como Fibre Channel, supone un transporte subyacente lossless.

## <a name="dcb-protocols-and-management-options"></a>Protocolos DCB y las opciones de administración

DCB está formado por el siguiente conjunto de protocolos. 

- Mejorado el servicio de transmisión \(ETS\) – IEEE 802.1Qaz, que se basa en la P 802.1X y 802.1Q estándares
- Controlar el flujo de prioridad \(PFS\), IEEE 802.1Qbb 
- Protocolo de intercambio DCB \(DCBX\), IEEE 802.1AB, como extendida en el 802.1Qaz estándar.

El protocolo DCBX te permite configurar DCB en un modificador, por lo que, a continuación, se puede configurar automáticamente un dispositivo de la final, como un equipo que ejecute Windows Server 2016.

Si prefieres administrar DCB desde un modificador, no tienes que instalar DCB como una característica de Windows Server 2016, pero este enfoque incluye algunas limitaciones.

Porque DCBX solo pueden informar al adaptador de red de host de tamaños de la clase de NET y habilitación PFC, sin embargo, hosts de Windows Server por lo general requieren que se instale DCB para que el tráfico se asigna a las clases de NET.

Normalmente, las aplicaciones de Windows no están diseñadas para participar en intercambios de DCBX. Por este motivo, el host debe estar configurado por separado de los conmutadores de red, pero con una configuración idéntica.

Si decides administrar DCB desde un modificador, aún puede ver las configuraciones en Windows Server 2016 mediante comandos de Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funcionalidad DCB importante

La siguiente es una lista que resume la funcionalidad proporcionada por DCB.

1. Proporciona la interoperabilidad entre los adaptadores de red compatibles con DCB\ y conmutadores DCB\ compatibles.

2. Proporciona un transporte Ethernet lossless entre un equipo que ejecute Windows Server 2016 y su conmutador más próximo al activar el control de flujo en función de priority\ en el adaptador de red.

3. Proporciona la capacidad para asignar el ancho de banda a un Control de tráfico \(TC\) por porcentaje, donde TC puede constar de una o varias clases de tráfico que se diferencian por indicadores de \(priority\) de clase de tráfico p 802.1X.

4. Permite que los administradores de servidor o los administradores de red asignar una aplicación a una clase de tráfico particular o prioridad en función de los protocolos conocidas, puerto TCP o UDP conocido o puerto NetworkDirect utilizada esa aplicación.

5. Proporciona administración de DCB a través de Windows Server 2016 Windows Management Instrumentation \(WMI\) y Windows PowerShell. Para obtener más información, consulta la sección [comandos de Windows PowerShell para DCB](#bkmk_wps) más adelante en este tema, además de los siguientes temas.
    - [Componentes DCB proporcionadas por el sistema](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [NDIS QoS requisitos para el puente de centro de datos](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Proporciona administración de DCB a través de la directiva de grupo de Windows Server 2016.

7. Admite la coexistencia de soluciones de calidad de servicio de Windows Server 2016 \(QoS\).

>[!NOTE]
>Antes de usar cualquier RDMA sobre la versión de \(RoCE\) convergido Ethernet de RDMA, debes habilitar DCB. Aunque no es necesario para las redes de protocolo de Internet amplia área RDMA \(iWARP\), pruebas ha determinado que todas las tecnologías de Ethernet\-based RDMA funcionan mejor con DCB. Por este motivo, considera la posibilidad de usar DCB para las implementaciones de RDMA iWARP. Para obtener más información, consulta [memoria de acceso directo remoto (RDMA) y cambiar incrustado agrupación (conjunto)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Aplicaciones prácticas de DCB

Muchas organizaciones tienen grandes Fibre Channel \(FC\) almacenamiento área \(SAN\) instalaciones de red para el servicio de almacenamiento. SAN FC requiere adaptadores de red especial de servidores y conmutadores FC en la red. Estas organizaciones normalmente también usan modificadores y adaptadores de red Ethernet.

En la mayoría de los casos, FC hardware es significativamente más costosa de implementar que hardware Ethernet, lo que genera los gastos de capital grandes. Además, el requisito de independiente Ethernet y SAN FC conmutador y adaptador de hardware de red requiere espacio adicional, potencia y plazo capacidad en el centro de datos, lo que genera los gastos de operaciones en curso, adicionales.

Desde una perspectiva de costo, resulta ventajoso para muchas organizaciones combinar su tecnología FC con su solución de hardware basado en Ethernet\ para proporcionar almacenamiento y servicios de red de datos.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Uso de DCB para un fabric convergente basados en Ethernet\ de almacenamiento y redes de datos

Para las organizaciones que ya tienen un gran SAN FC pero desea abandonar la inversión adicional en la tecnología FC, DCB que te permite crear una basada en Ethernet convergido fabric para el almacenamiento y redes de datos. Un DCB convergido fabric puede reducir el costo total futuro de propiedad \(TCO\) y simplificar la administración.

Para los hosts que ya han adoptado o que planear la adopción, iSCSI como su solución de almacenamiento, DCB puede proporcionar reserva asistida hardware\ ancho de banda de tráfico iSCSI para garantizar el aislamiento de rendimiento. Y a diferencia de otras soluciones propietarias, DCB basado en standards\ - y, por tanto, es relativamente fácil de implementar y administrar en un entorno de red heterogéneos.

Una implementación de Windows Server 2016\-based de DCB reduce muchos de los problemas que pueden surgir cuando convergido ofrecen soluciones de fabric varios \(OEMs\) fabricantes de equipos originales.

Las implementaciones de soluciones de propiedad proporcionadas por varios OEM no pueden interactuar con entre sí, puede ser difíciles administrar y generalmente tienen programaciones de mantenimiento de software diferentes. 

Por el contrario, Windows Server 2016 DCB está basado en standards\, y puedes implementar y administrar DCB en una red heterogénea.

## <a name="bkmk_wps"></a>Comandos de Windows PowerShell para DCB

Hay comandos DCB Windows PowerShell para Windows Server 2016 y Windows Server 2012 R2. Puedes usar todos los comandos de Windows Server 2012 R2 en Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos de PowerShell de Windows de Windows Server 2016 para DCB

El siguiente tema para Windows Server 2016 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los \(DCB\) puente de centro de datos calidad de servicio \ (QoS\) \-specific cmdlets. Enumera los cmdlets en orden alfabético según el verbo al principio del cmdlet.

- [Módulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandos de Windows Server 2012 R2 Windows PowerShell para DCB

El siguiente tema para Windows Server 2012 R2 proporciona descripciones de cmdlet de Windows PowerShell y la sintaxis para todos los \(DCB\) puente de centro de datos calidad de servicio \ (QoS\) \-specific cmdlets. Enumera los cmdlets en orden alfabético según el verbo al principio del cmdlet.

- [Centro de datos de calidad (DCB) de los Cmdlets de servicio (QoS) en Windows PowerShell de puente](https://technet.microsoft.com/library/hh967440.aspx)
