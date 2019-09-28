---
title: Conmutador virtual de Hyper-V
description: En este tema se proporciona información general sobre el conmutador virtual de Hyper-V en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c508005af67e9dd5b0c9a22693aca25eb19e8e48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366830"
---
# <a name="hyper-v-virtual-switch"></a>Conmutador virtual de Hyper-V

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información general sobre el conmutador virtual de Hyper-V, que ofrece la posibilidad de conectar máquinas virtuales \(VMs @ no__t-1 a redes externas al host de Hyper-no__t-2V, incluida la intranet de la organización e Internet. 

También puede conectarse a redes virtuales en el servidor que ejecuta Hyper @ no__t-0V al implementar redes definidas por software \(SDN @ no__t-2.

> [!NOTE]  
> Además de este tema, está disponible la siguiente documentación sobre el conmutador virtual de Hyper-V.  
>   
> - [Administración de conmutador virtual de Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Acceso directo a memoria remota (RDMA) y Switch Embedded Teaming (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Cmdlets del equipo del conmutador de red en Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [Novedades de VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configuración del tejido de red de VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Crear redes con VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V: Configuración de VLAN y etiquetado de VLAN @ no__t-0  
> - [Hyper-V: La extensión de conmutador virtual de WFP debe estar habilitada si lo requieren las extensiones de terceros @ no__t-0
>
> Para obtener más información acerca de otras tecnologías de red, consulte [funciones de red en Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
El conmutador virtual de Hyper @ no__t-0V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el administrador de Hyper @ no__t-1V al instalar el rol de servidor Hyper @ no__t-2V.

El conmutador virtual de Hyper-V incluye funcionalidades extensibles y administradas mediante programación para conectar máquinas virtuales tanto a redes virtuales como a la red física. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad.  
  
> [!NOTE]  
> El conmutador virtual de Hyper-V solo admite Ethernet y no es compatible con otras tecnologías de red de área local (LAN) por cable, como Infiniband y Canal de fibra.  
  
El conmutador virtual de Hyper-V incluye funcionalidades de aislamiento de inquilinos, formas de tráfico, protección frente a máquinas virtuales malintencionadas y solución de problemas simplificada. 

Gracias a la compatibilidad integrada con la especificación de interfaz de dispositivo de red \(NDIS @ no__t-1 los controladores de filtro y la plataforma de filtrado de Windows \(WFP @ no__t-3 controladores de llamada, el conmutador virtual de Hyper-V permite proveedores de software independientes \(ISVs @ No__ t-5 para crear complementos extensibles, denominados extensiones de conmutador virtual, que pueden proporcionar funciones mejoradas de red y seguridad. Las extensiones de conmutador virtual que agregue se enumerarán en la característica del Administrador de conmutadores virtuales del Administrador de Hyper-V.
  
En la siguiente ilustración, una máquina virtual tiene una NIC virtual que se conecta al conmutador virtual de Hyper-V a través de un puerto de conmutador.  
  
![Conexiones de conmutador virtual](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Las funcionalidades del conmutador virtual de Hyper-V proporcionan más opciones para aplicar el aislamiento de inquilinos, modelar y controlar el tráfico de red y emplear medidas de protección contra máquinas virtuales malintencionadas.

>[!NOTE]
> En Windows Server 2016, una máquina virtual con una NIC virtual muestra con precisión el rendimiento máximo de la NIC virtual. Para ver la velocidad de la NIC virtual en **conexiones de red**, haga clic con el botón derecho en el icono de NIC virtual que desee y, a continuación, haga clic en **Estado**. Se abre el cuadro de diálogo **Estado** de la NIC virtual. En **conexión**, el valor de **velocidad** coincide con la velocidad de la NIC física instalada en el servidor.
  
## <a name="bkmk_apps"></a>Usos del conmutador virtual de Hyper-V

A continuación se muestran algunos escenarios de casos de uso para el conmutador virtual de Hyper-V.

**Mostrar estadísticas**: un desarrollador de un proveedor de servicios hospedados en la nube implementa un paquete de administración que muestra el estado actual del conmutador virtual de Hyper-V. El paquete de administración puede consultar las funcionalidades actuales de todo el conmutador, las opciones de configuración y las estadísticas de redes de puertos individuales mediante WMI. Se muestra el estado del conmutador para proporcionar a los administradores una vista rápida de este.  
  
**Seguimiento de recursos**: una compañía de hospedaje vende servicios de hospedaje cuyo precio depende del nivel de pertenencia. Los distintos niveles de pertenencia incluyen niveles de rendimiento de red diferentes. El administrador asigna los recursos para cumplir con los contratos de nivel de servicio de modo tal que la disponibilidad de la red sea equilibrada. El administrador realiza un seguimiento mediante programación de la información, como el uso actual del ancho de banda asignado, y el número de canales de Virtual Machine Queue (VMQ) o de los canales de IOV asignados a la máquina virtual (VM). El mismo programa también registra mediante programación los recursos que se encuentran en uso, además de los recursos por VM asignados para recursos o seguimiento de entradas dobles.  
  
**Administrar el orden de las extensiones de conmutador**: una empresa instaló extensiones en su host de Hyper-V para supervisar el tráfico y notificar la detección de intrusiones. Durante el mantenimiento, es posible que, al actualizar algunas extensiones, se modifique su orden. Se ejecuta un programa de script simple para volver a ordenar las extensiones después de las actualizaciones.  
  
La **extensión de reenvío administra el ID. de VLAN**: una importante compañía de conmutadores crea una extensión de reenvío que aplica todas las directivas de redes. Uno de los elementos que se administran son los identificadores de red de área local virtual (VLAN). El conmutador virtual cede el control de la VLAN a una extensión de reenvío. La instalación de la compañía del conmutador llama mediante programación a una interfaz de programación de aplicaciones (API) de Instrumental de administración de Windows (WMI) que activa la transparencia, indicando al conmutador virtual de Hyper-V que pase y no realice ninguna acción en las etiquetas VLAN.  
  
## <a name="bkmk_func"></a>Funcionalidad de conmutador virtual de Hyper-V
 
A continuación, se enumeran algunas de las características principales incluidas en el conmutador virtual de Hyper-V:  
  
-   **Protección de envenenamiento (suplantación) ARP/ND**: proporciona protección contra una VM malintencionada que usa la suplantación de Protocolo de resolución de direcciones (ARP) para robar direcciones IP de otras VM. Proporciona protección contra ataques que pueden iniciarse para IPv6 mediante la suplantación de detección de equipos cercanos (ND).  
  
-   **Protección DHCP**: proporciona protección contra una VM malintencionada que se representa a sí misma como un servidor de Protocolo de configuración dinámica de host (DHCP) para ataques de tipo "Man in the middle".  
  
-   **ACL de Puerto**: proporciona filtrado de tráfico basado en intervalos o direcciones Media Access Control (MAC) o de protocolo de Internet (IP), lo que permite configurar el aislamiento de redes virtuales.  
  
-   **Modo de tronco en una máquina virtual**: permite a los administradores configurar una VM específica como un dispositivo virtual y, después, dirigir el tráfico de diversas VLAN a esa VM.  
  
-   **Supervisión del tráfico de red**: permite a los administradores revisar el tráfico que atraviesa el conmutador de red.  
  
-   **VLAN (privada) aislada**: permite a los administradores aislar tráfico en varias VLAN para establecer comunidades de inquilinos aislados con mayor facilidad.  
  
A continuación, se proporciona una lista de las funcionalidades que aumentan la facilidad de uso del conmutador virtual de Hyper-V:  
  
-   **Límite de ancho de banda y compatibilidad con ráfagas**: el ancho de banda mínimo garantiza la cantidad de ancho de banda reservado. El ancho de banda máximo limita la cantidad de ancho de banda que puede consumir una VM.  
  
-   **Compatibilidad con marcado de notificación explícita de congestión (ECN)** :  El marcado ECN, también conocido como protocolo TCP de datos (DCTCP), permite al conmutador físico y al sistema operativo regular el flujo de tráfico de modo que los recursos de búfer del conmutador no se congestionen, lo que aumenta el rendimiento del tráfico.  
  
-   **Diagnósticos**: los diagnósticos permiten el seguimiento y la supervisión sencillos de los eventos y paquetes del conmutador virtual.
