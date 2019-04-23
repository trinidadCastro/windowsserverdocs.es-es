---
title: Controladora de red
description: En este tema se proporciona información general de la controladora de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ace628c6ae9802c0c65d360aedfac8c80ac5537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875686"
---
# <a name="network-controller"></a>Controladora de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Nuevo en Windows Server 2016, controladora de red ofrece un punto centralizado programable de la automatización para administrar, configurar, supervisar y solucionar problemas de infraestructura de red virtual y física del centro de datos. 

Con Controladora de red puede automatizar la configuración de la infraestructura de red, en vez de tener que configurar de forma manual los dispositivos y servicios.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación de la controladora de red.
> - [Alta disponibilidad del controlador de red](network-controller-high-availability.md)
> - [Instalación y los requisitos de preparación para la implementación de controladora de red](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Implementar la controladora de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar el rol de servidor de controladora de red con el administrador del servidor](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Pasos posteriores a la implementación de controladora de red](post-deploy-steps-nc.md)
> - [Cmdlets de controlador de red](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Resumen de controladora de red

Controladora de red es un rol de servidor altamente disponible y escalable y proporciona una interfaz de programación de aplicación \(API\) que permite que la controladora de red para comunicarse con la red y una segunda API que le permite comunicarse con controladora de red.

Puede implementar la controladora de red en el dominio y los entornos que no sea de dominio. En entornos de dominio, controlador de red autentica a usuarios y dispositivos de red mediante el uso de Kerberos; en entornos que no son de dominio, debe implementar certificados para la autenticación.

>[!IMPORTANT]
>No implemente el rol de servidor de controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de controladora de red en una máquina virtual de Hyper-V \(VM\) que está instalado en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en Hyper diferentes tres\-hosts V, debe habilitar Hyper\-hosts V para redes definidas por Software \(SDN\) mediante la adición de los hosts al uso de la controladora de red el comando de Windows PowerShell **New NetworkControllerServer**. Al hacerlo, permite que el equilibrador de carga de Software de SDN a función. Para obtener más información, consulte [New NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Controladora de red se comunica con dispositivos, servicios y componentes de red mediante la API Southbound. Esta API permite a Controladora de red detectar dispositivos de red y configuraciones de servicios, así como obtener toda la información necesaria sobre la red. Además, la API Southbound ofrece un canal para enviar información a la infraestructura de red, como los cambios de configuración que se realicen.

La API Northbound le permite obtener información de Controladora de red y usarla para supervisar y configurar la red.

La API Northbound de controladora de red le permite configurar, supervisar, solucionar problemas e implementar nuevos dispositivos en la red mediante Windows PowerShell, la transferencia de estado representacional \(REST\) API o una aplicación de administración con una interfaz gráfica de usuario, como System Center Virtual Machine Manager.

>[!NOTE]
>La API Northbound de Controladora de red se implementa como una interfaz REST.

Puede administrar la red de centro de datos con controladora de red mediante el uso de las aplicaciones de administración, como System Center Virtual Machine Manager \(SCVMM\)y System Center Operations Manager \(SCOM\), Dado que el controlador de red le permite configurar, supervisar, programar y solucionar problemas de la infraestructura de red que está bajo su control.

Mediante Windows PowerShell, la API REST o una aplicación de administración, puede usar Controladora de red para administrar la siguiente infraestructura de red física y virtual:

- MV Hyper-V y conmutadores virtuales

- Firewall de centro de datos

- Servicio de acceso remoto \(RAS\) grupos de puerta de enlace, puertas de enlace Virtual y las puertas de enlace para varios inquilinos

- Equilibradores de carga de software

En la siguiente ilustración, un administrador usa una herramienta de administración que interactúa directamente con Controladora de red. Controladora de red proporciona información acerca de la infraestructura de red, incluida la infraestructura virtual y física, a la herramienta de administración y realiza cambios de configuración según las acciones que el administrador al usar la herramienta.  

![Resumen de controladora de red](../../../media/Network-Controller/NetController_overview.png)  

Si va a implementar la controladora de red en un entorno de laboratorio de pruebas, puede ejecutar el rol de servidor de controladora de red en una máquina virtual de Hyper-V \(VM\) que está instalado en un host de Hyper-V.

Para lograr alta disponibilidad en centros de datos grandes, puede implementar un clúster mediante el uso de tres máquinas virtuales que están instaladas en tres o más hosts de Hyper-V. Para obtener más información, consulte [alta disponibilidad del controlador de red](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Características de controladora de red

Las siguientes características de Controladora de red le permiten configurar y administrar dispositivos y servicios de red físicos y virtuales.  
  
-   [Administración de Firewall](#bkmk_firewall)  
  
-   [Administración del equilibrador de carga de software](#bkmk_slb)  
  
-   [Administración de red virtual](#bkmk_virtual)  
  
-   [Administración de puerta de enlace RAS](#bkmk_gateway)

>[!IMPORTANT]
>Copia de seguridad de controladora de red y la restauración no está disponible actualmente en Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Administración de Firewall

Esta característica de Controladora de red le permite configurar y administrar reglas de Access Control (permitir/denegar) del firewall en las MV de carga de trabajo, tanto en el tráfico de red este/oeste y como en el norte/sur de su centro de datos. Las reglas de firewall se aplican en el puerto vSwitch de las MV de carga de trabajo, por lo que se distribuyen por la carga de trabajo en el centro de datos. Con la API Northbound puede definir las reglas de firewall para el tráfico entrante y saliente de la MV de carga de trabajo. También puede configurar cada regla de firewall para que registre el tráfico al que permite o deniega el paso.  

Para obtener más información, consulte [información general de Datacenter Firewall](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Administración del equilibrador de carga de software

Esta característica de red le permite habilitar múltiples servidores para que alojen la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.  
  
Para obtener más información, consulte [equilibrio de carga de Software &#40;SLB&#41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Administración de red virtual

Esta característica de Controladora de red le permite implementar y configurar en MV individuales la virtualización de red Hyper-V, lo que incluye el Conmutador virtual de Hyper-V y adaptadores de red virtuales, así como almacenar y distribuir directivas de red virtual.

Controladora de red es compatible con Network Virtualization Generic Routing Encapsulation (NVGRE) y Virtual Extensible Local Area Network (VXLAN).

### <a name="bkmk_gateway"></a>Administración de puerta de enlace RAS

Esta característica de controladora de red permite implementar, configurar y administrar máquinas virtuales (VM) que son miembros de un grupo de servidores de puerta de enlace RAS, que proporciona servicios de puerta de enlace a los inquilinos. Controladora de red le permite implementar automáticamente las máquinas virtuales que ejecutan la puerta de enlace de RAS con las siguientes características de la puerta de enlace:

> [!NOTE]
> En System Center Virtual Machine Manager, la puerta de enlace de RAS se denomina puerta de enlace de Windows Server.

- Agregar y eliminar del clúster MV de puerta de enlace y especificar el nivel necesario de copia de seguridad.

- Conectividad de puerta de enlace de red privada virtual (VPN) sitio a sitio entre las redes de inquilinos remotos y el centro de datos mediante IPsec.

- Conectividad de puerta de enlace VPN sitio a sitio entre las redes de inquilinos remotos y el centro de datos mediante encapsulación de enrutamiento genérico (GRE).

- Capacidad de reenvío de nivel 3.

- Border Gateway Protocol (BGP) de enrutamiento, que le permite administrar el enrutamiento de tráfico de red entre redes de máquinas virtuales de los inquilinos y sus sitios remotos.

Controladora de red puede colocar las diferentes conexiones de un inquilino en puertas de enlace independientes. Puede usar una sola dirección IP pública para todas las conexiones de puerta de enlace o tienen diferentes direcciones IP públicas para un subconjunto de las conexiones. Controladora de red registra toda la configuración de puerta de enlace y los cambios de estado, que se pueden usar para solucionar problemas y auditoría.

Para obtener más información sobre BGP, consulte [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Para obtener más información sobre la puerta de enlace de RAS, consulte [puerta de enlace RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opciones de implementación del controlador de red

Para implementar la controladora de red mediante System Center Virtual Machine Manager \(VMM\), consulte [configurar un controlador de red de SDN en el tejido de VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implementar la controladora de red con las secuencias de comandos, consulte [implementar un Software define infraestructura usando los Scripts de red](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implementar la controladora de red mediante Windows PowerShell, consulte [implementar controladora de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
