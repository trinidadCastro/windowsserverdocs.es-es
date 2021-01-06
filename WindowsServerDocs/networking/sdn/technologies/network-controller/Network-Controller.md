---
title: Controladora de red
description: En este tema se proporciona información general de la controladora de red en Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: anpaul
author: AnirbanPaul
ms.date: 12/08/2020
ms.openlocfilehash: 8db1514711834430b88a94306189ce03c0036899
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949401"
---
# <a name="network-controller"></a>Controladora de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Como novedad en Windows Server 2016, la controladora de red proporciona un punto de automatización programable y centralizado para administrar, configurar, supervisar y solucionar problemas de la infraestructura de red virtual y física en el centro de recursos.

Con Controladora de red puede automatizar la configuración de la infraestructura de red, en vez de tener que configurar de forma manual los dispositivos y servicios.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación de la controladora de red.
> - [Alta disponibilidad de Controladora de red](network-controller-high-availability.md)
> - [Requisitos de instalación y preparación para la implementación de la controladora de red](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)
> - [Implementación de controladora de red con Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
> - [Instalación del rol de servidor de Controladora de red mediante el Administrador del servidor](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Pasos posteriores a la implementación de la controladora de red](post-deploy-steps-nc.md)
> - [Cmdlets de controladora de red](/powershell/module/networkcontroller/)

## <a name="network-controller-overview"></a><a name="bkmk_overview"></a>Información general de la controladora de red

La controladora de red es un rol de servidor escalable y de alta disponibilidad, y proporciona una API de interfaz de programación de aplicaciones \( \) que permite que la controladora de red se comunique con la red y una segunda API que le permite comunicarse con la controladora de red.

Puede implementar Controladora de red en entornos de dominio y en entornos que no pertenecen a un dominio. En entornos de dominio, Controladora de red autentica los usuarios y dispositivos de red mediante Kerberos. En entornos que no son de dominio, debe implementar certificados para la autenticación.

>[!IMPORTANT]
>No implemente el rol del servidor Controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \( \) que esté instalada en un host de Hyper-v. Una vez que haya instalado el controlador de red en las máquinas virtuales en tres hosts de Hyper- \- v diferentes, debe habilitar los \- hosts de Hyper v para las redes definidas por software de \( Sdn \) agregando los hosts a la controladora de red mediante el comando de Windows PowerShell **New-NetworkControllerServer**. Al hacerlo, permite funcionar al equilibrador de carga de las redes definidas por software. Para obtener más información, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Controladora de red se comunica con dispositivos, servicios y componentes de red mediante la API Southbound. Esta API permite a Controladora de red detectar dispositivos de red y configuraciones de servicios, así como obtener toda la información necesaria sobre la red. Además, la API Southbound ofrece un canal para enviar información a la infraestructura de red, como los cambios de configuración que se realicen.

La API Northbound le permite obtener información de Controladora de red y usarla para supervisar y configurar la red.

La API Northbound de la controladora de red le permite configurar, supervisar, solucionar problemas e implementar nuevos dispositivos en la red mediante Windows PowerShell, la API de REST de transferencia de estado representacional \( \) o una aplicación de administración con una interfaz gráfica de usuario, como System Center Virtual Machine Manager.

>[!NOTE]
>La API Northbound de Controladora de red se implementa como una interfaz REST.

Puede administrar la red del centro de recursos con la controladora de red mediante aplicaciones de administración, como System Center Virtual Machine Manager \( SCVMM \) y System Center Operations Manager \( SCOM \) , ya que el controlador de red permite configurar, supervisar, programar y solucionar problemas de la infraestructura de red que se encuentra bajo su control.

Mediante Windows PowerShell, la API REST o una aplicación de administración, puede usar Controladora de red para administrar la siguiente infraestructura de red física y virtual:

- MV Hyper-V y conmutadores virtuales

- Firewall de centro de datos

- Puertas de enlace para varios \( inquilinos ras de servicio de acceso remoto \) , puertas de enlace virtuales y grupos de puerta de enlace

- Equilibradores de carga de software

En la siguiente ilustración, un administrador usa una herramienta de administración que interactúa directamente con Controladora de red. La controladora de red proporciona información acerca de la infraestructura de red, incluida la infraestructura física y virtual, a la herramienta de administración y realiza cambios de configuración en función de las acciones del administrador cuando se usa la herramienta.

![Introducción a la controladora de red](../../../media/Network-Controller/NetController_overview.png)

Si va a implementar un controlador de red en un entorno de laboratorio de pruebas, puede ejecutar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \( \) que esté instalada en un host de Hyper-v.

Para lograr una alta disponibilidad en centros de recursos de mayor tamaño, puede implementar un clúster con tres máquinas virtuales que estén instaladas en tres o más hosts de Hyper-V. Para obtener más información, consulte [alta disponibilidad de la controladora de red](network-controller-high-availability.md).

## <a name="network-controller-features"></a><a name="bkmk_features"></a>Características de la controladora de red

Las siguientes características de Controladora de red le permiten configurar y administrar dispositivos y servicios de red físicos y virtuales.

-   [Administración de Firewall](#bkmk_firewall)

-   [Administración de Load Balancer de software](#bkmk_slb)

-   [Administración de Virtual Network](#bkmk_virtual)

-   [Administración de puerta de enlace RAS](#bkmk_gateway)

>[!IMPORTANT]
>La copia de seguridad y restauración de la controladora de red no está disponible actualmente en Windows Server 2016.

### <a name="firewall-management"></a><a name="bkmk_firewall"></a>Administración de Firewall

Esta característica de Controladora de red le permite configurar y administrar reglas de Access Control (permitir/denegar) del firewall en las MV de carga de trabajo, tanto en el tráfico de red este/oeste y como en el norte/sur de su centro de datos. Las reglas de firewall se aplican en el puerto vSwitch de las MV de carga de trabajo, por lo que se distribuyen por la carga de trabajo en el centro de datos. Con la API Northbound puede definir las reglas de firewall para el tráfico entrante y saliente de la MV de carga de trabajo. También puede configurar cada regla de firewall para que registre el tráfico al que permite o deniega el paso.

Para obtener más información, consulte [información general sobre el firewall del centro](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)de datos.

### <a name="software-load-balancer-management"></a><a name="bkmk_slb"></a>Administración de Load Balancer de software

Esta característica de red le permite habilitar múltiples servidores para que alojen la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

Para obtener más información, consulte [equilibrio de carga de Software &#40;SLB&#41; para Sdn](../network-function-virtualization/software-load-balancing-for-sdn.md).

### <a name="virtual-network-management"></a><a name="bkmk_virtual"></a>Administración de Virtual Network

Esta característica de Controladora de red le permite implementar y configurar en MV individuales la virtualización de red Hyper-V, lo que incluye el Conmutador virtual de Hyper-V y adaptadores de red virtuales, así como almacenar y distribuir directivas de red virtual.

Controladora de red es compatible con Network Virtualization Generic Routing Encapsulation (NVGRE) y Virtual Extensible Local Area Network (VXLAN).

### <a name="ras-gateway-management"></a><a name="bkmk_gateway"></a>Administración de puerta de enlace RAS

Esta característica de controladora de red le permite implementar, configurar y administrar máquinas virtuales (VM) que son miembros de un grupo de puerta de enlace de RAS, proporcionando servicios de puerta de enlace a los inquilinos. La controladora de red le permite implementar automáticamente máquinas virtuales que ejecutan la puerta de enlace RAS con las siguientes características de puerta de enlace:

> [!NOTE]
> En System Center Virtual Machine Manager, la puerta de enlace RAS se denomina puerta de enlace de Windows Server.

- Agregar y eliminar del clúster MV de puerta de enlace y especificar el nivel necesario de copia de seguridad.

- Conectividad de puerta de enlace de red privada virtual (VPN) sitio a sitio entre las redes de inquilinos remotos y el centro de datos mediante IPsec.

- Conectividad de puerta de enlace VPN sitio a sitio entre las redes de inquilinos remotos y el centro de datos mediante encapsulación de enrutamiento genérico (GRE).

- Capacidad de reenvío de nivel 3.

- Enrutamiento de Protocolo de puerta de enlace de borde (BGP), que permite administrar el enrutamiento del tráfico de red entre las redes de VM de los inquilinos y sus sitios remotos.

La controladora de red puede colocar diferentes conexiones de un inquilino en puertas de enlace independientes. Puede usar una única dirección IP pública para todas las conexiones de puerta de enlace o tener diferentes direcciones IP públicas para un subconjunto de las conexiones. La controladora de red registra todos los cambios de estado y configuración de puerta de enlace, que se pueden usar para la auditoría y la solución de problemas.

Para obtener más información sobre BGP, consulte [Protocolo de puerta de enlace de borde &#40;&#41;BGP ](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Para obtener más información sobre la puerta de enlace RAS, consulte [puerta de enlace ras para Sdn](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opciones de implementación de la controladora de red

Para implementar la controladora de red mediante System Center Virtual Machine Manager \( VMM \) , consulte [configuración de una controladora de red de Sdn en el tejido de VMM](/system-center/vmm/sdn-controller).

Para implementar la controladora de red mediante scripts, consulte [implementación de una infraestructura de red definida por software mediante scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implementar la controladora de red con Windows PowerShell, consulte implementación de la [controladora de red con Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md) .
