---
title: Conmutador virtual de Hyper-V
description: En este tema se proporciona información general de conmutador Virtual de Hyper-V en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17962b9bbb90a5ea1b6a4a303c64d72f74be37b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860926"
---
# <a name="hyper-v-virtual-switch"></a>Conmutador virtual de Hyper-V

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema proporciona información general sobre el conmutador Virtual de Hyper-V, que proporciona la capacidad para conectar máquinas virtuales \(máquinas virtuales\) a redes externas a Hyper\-host V, incluida la intranet de su organización e Internet. 

También puede conectarse a redes virtuales en el servidor que ejecuta Hyper\-V cuando se implementan redes definidas por Software \(SDN\).

> [!NOTE]  
> Además de este tema, la siguiente documentación de conmutador Virtual de Hyper-V está disponible.  
>   
> - [Administrar el conmutador Virtual de Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Acceso de memoria directa remota (RDMA) y Switch Embedded Teaming (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Cmdlets de equipo de conmutadores de red en Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [Novedades de VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurar el tejido de red de VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Crear redes con VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V: Configurar las VLAN y etiquetado de VLAN](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V: La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> Para obtener más información acerca de otras tecnologías de red, consulte [a redes en Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
Hyper\-V Virtual Switch es un basada en software de capa 2 Ethernet conmutador de red que está disponible en Hyper\-V Manager cuando se instala Hyper\-rol de servidor V.

Conmutador Virtual de Hyper-V incluye funcionalidades extensibles y administradas mediante programación para conectar las máquinas virtuales a las redes virtuales y la red física. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad.  
  
> [!NOTE]  
> El conmutador virtual de Hyper-V solo admite Ethernet y no es compatible con otras tecnologías de red de área local (LAN) por cable, como Infiniband y Canal de fibra.  
  
Conmutador Virtual de Hyper-V incluye capacidades de aislamiento de inquilinos, la catalogación del tráfico, la protección contra máquinas virtuales malintencionadas y simplifica la solución de problemas. 

Con compatibilidad integrada para la especificación de interfaz de dispositivo de red \(NDIS\) filtrar controladores y plataforma de filtrado de Windows \(WFP\) controladores de llamadas, permite que el conmutador Virtual de Hyper-V independientes de software los proveedores \(ISV\) para crear complementos extensibles, llama como las extensiones de conmutador Virtual, que puede proporcionar capacidades mejoradas de redes y seguridad. Las extensiones de conmutador virtual que agregue se enumerarán en la característica del Administrador de conmutadores virtuales del Administrador de Hyper-V.
  
En la siguiente ilustración, una máquina virtual tiene una NIC virtual que está conectada al conmutador Virtual de Hyper-V a través de un puerto de conmutador.  
  
![Conexiones del conmutador virtuales](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Capacidades del conmutador Virtual de Hyper-V proporcionan más opciones para aplicar el aislamiento de inquilinos, catalogar y controlar tráfico de red, y emplear medidas de protección contra máquinas virtuales malintencionadas.

>[!NOTE]
> En Windows Server 2016, una máquina virtual con una NIC virtual muestra con precisión el rendimiento máximo para la NIC virtual. Para ver la velocidad NIC virtual en **las conexiones de red**, haga clic en el icono NIC virtual deseado y, a continuación, haga clic en **estado**. La NIC virtual **estado** abre el cuadro de diálogo. En **conexión**, el valor de **velocidad** coincide con la velocidad de la NIC física instalada en el servidor.
  
## <a name="bkmk_apps"></a>Usos de conmutador Virtual de Hyper-V

Siguientes son algunos escenarios de casos de uso para Hyper-V Virtual Switch.

**Mostrar estadísticas**: un desarrollador de un proveedor de servicios hospedados en la nube implementa un paquete de administración que muestra el estado actual del conmutador virtual de Hyper-V. El paquete de administración puede consultar las funcionalidades actuales de todo el conmutador, las opciones de configuración y las estadísticas de redes de puertos individuales mediante WMI. Se muestra el estado del conmutador para proporcionar a los administradores una vista rápida de este.  
  
**Seguimiento de recursos**: una compañía de hospedaje vende servicios de hospedaje cuyo precio depende del nivel de pertenencia. Los distintos niveles de pertenencia incluyen niveles de rendimiento de red diferentes. El administrador asigna los recursos para cumplir con los contratos de nivel de servicio de modo tal que la disponibilidad de la red sea equilibrada. El administrador realiza un seguimiento mediante programación información como el uso actual del ancho de banda asignado y el número de la máquina virtual (VM) asignadas de virtual machine queue (VMQ) o canales IOV. El mismo programa también registra mediante programación los recursos que se encuentran en uso, además de los recursos por VM asignados para recursos o seguimiento de entradas dobles.  
  
**Administración del orden de las extensiones de conmutador**: una empresa instaló extensiones en su host de Hyper-V para supervisar el tráfico y notificar la detección de intrusiones. Durante el mantenimiento, es posible que, al actualizar algunas extensiones, se modifique su orden. Se ejecuta un programa de script simple para volver a ordenar las extensiones después de las actualizaciones.  
  
**Extensión de reenvío Id. de VLAN**: una importante compañía de conmutadores crea una extensión de reenvío que aplica todas las directivas de redes. Uno de los elementos que se administran son los identificadores de red de área local virtual (VLAN). El conmutador virtual cede el control de la VLAN a una extensión de reenvío. Instalación de la compañía de conmutadores llamar mediante programación a una interfaz de programación de aplicaciones (API) del Instrumental de administración de Windows (WMI) que activa la transparencia, indicando al conmutador Virtual de Hyper-V para pasar y no realice ninguna acción en las etiquetas VLAN.  
  
## <a name="bkmk_func"></a>Funcionalidad de conmutador Virtual de Hyper-V
 
A continuación, se enumeran algunas de las características principales incluidas en el conmutador virtual de Hyper-V:  
  
-   **Protección de ARP/ND "poisoning" (suplantación de identidad)**: proporciona protección contra una VM malintencionada que usa la suplantación de Protocolo de resolución de direcciones (ARP) para robar direcciones IP de otras VM. Proporciona protección contra ataques que pueden iniciarse para IPv6 mediante la suplantación de detección de equipos cercanos (ND).  
  
-   **Protección DHCP**: proporciona protección contra una VM malintencionada que se representa a sí misma como un servidor de Protocolo de configuración dinámica de host (DHCP) para ataques de tipo "Man in the middle".  
  
-   **ACL de puerto**: proporciona filtrado de tráfico basado en intervalos o direcciones Media Access Control (MAC) o de protocolo de Internet (IP), lo que permite configurar el aislamiento de redes virtuales.  
  
-   **Modo de tronco para una máquina virtual**: permite a los administradores configurar una VM específica como un dispositivo virtual y, después, dirigir el tráfico de diversas VLAN a esa VM.  
  
-   **Supervisión del tráfico de red**: permite a los administradores revisar el tráfico que atraviesa el conmutador de red.  
  
-   **VLAN (privada) aislada**: permite a los administradores aislar tráfico en varias VLAN para establecer comunidades de inquilinos aislados con mayor facilidad.  
  
A continuación, se proporciona una lista de las funcionalidades que aumentan la facilidad de uso del conmutador virtual de Hyper-V:  
  
-   **Compatibilidad con ráfaga y límite de ancho de banda**: el ancho de banda mínimo garantiza la cantidad de ancho de banda reservado. El ancho de banda máximo limita la cantidad de ancho de banda que puede consumir una VM.  
  
-   **Notificación de congestión explícita (ECN), compatibilidad con marcado**:  ECN marcar, también conocido como datos de protocolo TCP de centro (DCTCP), permite que el conmutador físico y sistema operativo regular el flujo de tráfico de modo que los recursos de búfer del conmutador no se congestionen, lo que el rendimiento de un aumento de tráfico.  
  
-   **Diagnósticos**: los diagnósticos permiten el seguimiento y la supervisión sencillos de los eventos y paquetes del conmutador virtual.
