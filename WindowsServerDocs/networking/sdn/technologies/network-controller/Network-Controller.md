---
title: Controlador de red
description: Este tema proporciona una visión general de controlador de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: acb9abbd716e9930fb01431e7004abb72a7da10c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller"></a>Controlador de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Novedad en Windows Server 2016, controlador de red proporciona un punto centralizado, programable de la automatización para administrar, configurar, supervisar y solucionar problemas de infraestructura de red físicas y virtuales en el centro de datos. 

Usar el controlador de red, puede automatizar la configuración de la infraestructura de red en lugar de realizar la configuración manual de dispositivos de red y servicios.

> [!NOTE]
> Además de este tema, la siguiente documentación de controlador de red está disponible.
> - [Alta disponibilidad del controlador de red](network-controller-high-availability.md)
> - [Instalación y requisitos de preparación para implementar el controlador de red](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Implementar el controlador de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar el rol de servidor de controlador de red mediante el administrador del servidor](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [POST-Post-Deployment Pasos para el controlador de red](post-deploy-steps-nc.md)
> - [Cmdlets de controlador de red](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Información general de controlador de red

Controlador de red es un rol de servidor altamente disponible y escalable y proporciona una programación de aplicaciones de la interfaz \(API\) que permite que el controlador de red para comunicarse con la red y una segunda API que se puede comunicar con el controlador de red.

Puedes implementar el controlador de red de dominio y entornos no del dominio. En entornos de dominio, el controlador de red autentica a los usuarios y dispositivos de red con Kerberos; en entornos no del dominio, debes implementar certificados para la autenticación.

>[!IMPORTANT]
>No se implementará el rol de servidor de controlador de red en hosts físicos. Para implementar el controlador de red, debes instalar el rol de servidor de controlador de red en una máquina virtual de Hyper-V \(VM\) que está instalada en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en tres distintos hosts Hyper\-V, debe habilitar los hosts Hyper\-V para Software definido redes \(SDN\) agregando los hosts en los controladores de red mediante el comando de Windows PowerShell **nueva NetworkControllerServer**. Al hacerlo, se habilita el equilibrado de carga de Software SDN a función. Para obtener más información, consulta [nueva NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Controlador de red se comunica con dispositivos de red, servicios y componentes mediante la API Southbound. Con la API Southbound, controlador de red puede detectar dispositivos de red, configuraciones del servicio de detectar y reunir toda la información que necesitas acerca de la red. Además, la API Southbound ofrece controlador de red de una ruta para enviar información a la infraestructura de red, como cambios de configuración que has hecho.

La API Northbound del controlador de red proporciona la capacidad para recopilar información de red desde el controlador de red y se usa para supervisar y configurar la red.

La API Northbound del controlador de red te permite configurar, supervisar, solucionar problemas e implementar nuevos dispositivos en la red mediante Windows PowerShell, la API de transferencia de estado representacional \(REST\) o una aplicación de administración con una interfaz gráfica de usuario, como System Center Virtual Machine Manager.

>[!NOTE]
>La API Northbound del controlador de red se implementa como una interfaz REST.

Puedes administrar la red de centros de datos con el controlador de red mediante el uso de aplicaciones de administración, como System Center Virtual Machine Manager \(SCVMM\) y System Center Operations Manager \(SCOM\), porque el controlador de red te permite configurar, supervisar, programa y solucionar problemas de la infraestructura de red que está bajo su control.

Con Windows PowerShell, la API de REST o una aplicación de administración, puedes usar el controlador de red para administrar la infraestructura de red físicas y virtuales siguientes:

- Máquinas virtuales de Hyper-V y conmutadores virtuales

- Firewall de centros de datos

- Puertas de enlace Multitenant \(RAS\) de servicio de acceso remoto, puertas de enlace Virtual y conjuntos de puerta de enlace

- Equilibradores de carga de software

En la siguiente ilustración, un administrador usa una herramienta de administración que interactúa directamente con el controlador de red. Controlador de red proporciona información sobre la infraestructura de red, incluida la infraestructura virtual y física, en la herramienta de administración y realiza los cambios de configuración según las acciones al usar la herramienta del administrador.  

![Introducción al controlador de red](../../../media/Network-Controller/NetController_overview.png)  

Si vas a implementar el controlador de red en un entorno de laboratorio de prueba, puedes ejecutar el rol de servidor de controlador de red en una máquina virtual de Hyper-V \(VM\) que está instalada en un host de Hyper-V.

Para una alta disponibilidad en centros de datos más grandes, puede implementar un clúster mediante tres máquinas virtuales que están instaladas en los hosts de Hyper-V tres o más. Para obtener más información, consulta [alta disponibilidad del controlador de red](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Características de controlador de red

Las siguientes funciones de controlador de red permiten configurar y administrar virtual y dispositivos de red física y servicios.  
  
-   [Administración del Firewall](#bkmk_firewall)  
  
-   [Administración de equilibrado de carga de software](#bkmk_slb)  
  
-   [Administración de red virtual](#bkmk_virtual)  
  
-   [Administración de puerta de enlace RAS](#bkmk_gateway)

>[!IMPORTANT]
>Copia de seguridad de controlador de red y restauración no está disponible actualmente en Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Administración del Firewall

Esta función de controlador de red te permite configurar y administrar permitir o denegar las reglas de Control de acceso de firewall para la carga de trabajo virtuales para este y Oeste y el tráfico de red norte/sur en el centro de datos. Las reglas de firewall que se conectan en el puerto vSwitch de máquinas virtuales de carga de trabajo y, por lo tanto, se distribuyen a través de la carga de trabajo en el centro de datos. Con la API Northbound, puedes definir las reglas de firewall para el tráfico entrante y saliente de la máquina virtual de carga de trabajo. También puedes configurar cada regla de firewall para registrar el tráfico que se permite o deniega la regla.  

Para obtener más información, consulta [Datacenter Firewall Introducción](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Administración de equilibrado de carga de software

Esta función de controlador de red te permite habilitar varios servidores hospedar la misma carga de trabajo que escalabilidad y alta disponibilidad.  
  
Para obtener más información, consulta [equilibrio de carga de Software & #40; SLB & #41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Administración de red virtual

Esta función de controlador de red permite implementar y configurar la virtualización de Hyper-V en la red, incluido el conmutador Virtual de Hyper-V y adaptadores de red virtual en máquinas virtuales individuales y almacenar y distribuir las directivas de red virtual.

Controlador de red admite red virtualización genérico enrutamiento encapsulación (NVGRE) y red de área Local Extensible Virtual (VXLAN).

### <a name="bkmk_gateway"></a>Administración de puerta de enlace RAS

Esta función de controlador de red permite implementar, configurar y administrar las máquinas virtuales (VM) que son miembros de un conjunto de puerta de enlace de RAS, proporcionar servicios de puerta de enlace a tus inquilinos. Controlador de red permite implementar automáticamente las máquinas virtuales que ejecutan la puerta de enlace de RAS con las siguientes características de puerta de enlace:

> [!NOTE]
> En System Center Virtual Machine Manager, puerta de enlace de RAS se denomina puerta de enlace de servidor de Windows.

- Agregar y quitar del clúster máquinas virtuales de puerta de enlace y especificar el nivel de copia de seguridad necesario.

- Conectividad de puerta de enlace de red privada virtual (VPN) de sitio a sitio entre redes inquilino remoto y el centro de datos con IPsec.

- Conectividad de puerta de enlace de sitio a sitio VPN entre redes inquilino remoto y el centro de datos con encapsulación de enrutamiento genérico (GRE).

- De nivel 3 de reenvío de funcionalidad.

- Borde Gateway Protocol (BGP) enrutamiento, que le permite administrar el enrutamiento de tráfico de red entre redes de máquina virtual de su los de inquilinos y sus sitios remotos.

Controlador de red puede colocar diferentes conexiones de un inquilino en puertas de enlace independientes. Puedes usar una dirección IP pública único para todas las conexiones de puerta de enlace o tener varias direcciones IP pública para un subconjunto de las conexiones. Controlador de red registra todos los configuración puerta de enlace y cambios de estado, que pueden usarse para la auditoría y solucionar problemas.

Para obtener más información sobre BGP, consulta [protocolo de enlace de borde & #40; BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Para obtener más información sobre la puerta de enlace de RAS, consulta [RAS puerta de enlace SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opciones de implementación del controlador de red

Para implementar el controlador de red mediante el uso de System Center Virtual Machine Manager \(VMM\), consulta [configurar un controlador de red SDN en la estructura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implementar el controlador de red mediante scripts, consulta [implementar un Software definido red infraestructura usar Scripts de](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implementar el controlador de red mediante Windows PowerShell, consulta [implementar controladores de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
