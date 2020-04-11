---
title: Servicios de Azure y consideraciones para el hospedaje de escritorio
description: Conoce aspectos exclusivos de Azure con una solución de hospedaje de Escritorio remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: lizross
ms.openlocfilehash: f73f28500c136ec8bdd32084cc5949f5e9804699
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818528"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Servicios de Azure y consideraciones para el hospedaje de escritorio

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Las secciones siguientes describen los Servicios de infraestructura de Azure.
  
## <a name="azure-portal"></a>Azure Portal

Después de que el proveedor crea una suscripción de Azure, se puede usar Azure Portal para crear manualmente cada entorno del inquilino. Este proceso también se puede automatizar mediante scripts de PowerShell.  

Para más información, visita el sitio web de [Microsoft Azure](https://www.azure.microsoft.com).
  
## <a name="azure-load-balancer"></a>Azure Load Balancer

Los componentes del inquilino se ejecutan en máquinas virtuales que se comunican entre sí en una red aislada. Durante el proceso de implementación, puedes acceder externamente a estas máquinas virtuales mediante Azure Load Balancer con los puntos de conexión del Protocolo de escritorio remoto o un punto de conexión de PowerShell remoto. Una vez completada una implementación, normalmente se eliminarán estos puntos de conexión para reducir la superficie expuesta a ataques. Los únicos puntos de conexión serán los puntos de conexión HTTPS y UDP que se crearon para la máquina virtual en la que se ejecutaban los componentes de acceso web y puerta de enlace de Escritorio remoto. Esto permite a los clientes de Internet conectarse a las sesiones que se ejecutan en el servicio de hospedaje de escritorio del inquilino. Si un usuario abre una aplicación que se conecta a Internet, como un explorador web, las conexiones se pasarán a través de Azure Load Balancer.  
  
Para más información, consulta [¿Qué es Azure Load Balancer?](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>Consideraciones de seguridad

Esta guía de arquitectura de referencia de hospedaje de escritorio de Azure está diseñada para proporcionar un entorno altamente seguro y aislado para cada inquilino. La seguridad del sistema depende también de las medidas de seguridad que tome el proveedor durante la implementación y funcionamiento del servicio hospedado. La lista siguiente indica algunos aspectos que el proveedor debería tener en cuenta para proteger su solución de hospedaje de escritorio basada en esta arquitectura de referencia.

- Todas las contraseñas administrativas deben ser seguras, se deberían generar de forma aleatoria (idealmente), cambiarse frecuentemente y guardarse en una ubicación central a la que solo puedan acceder un selecto grupo de administradores del proveedor.  
- Cuando repliques el entorno del inquilino para los nuevos inquilinos, evite usar la misma contraseña o contraseñas administrativas no seguras.
- La dirección URL, el nombre y los certificados del sitio de acceso web de Escritorio remoto deben ser únicos y reconocibles para cada inquilino para evitar ataques de suplantación de identidad.  
- Durante el funcionamiento normal del servicio de hospedaje de escritorio, se deben eliminar todas las direcciones IP públicas de todas las máquinas virtuales excepto de la máquina virtual de acceso web y de puerta de enlace de Escritorio remoto que permite a los usuarios conectarse de forma segura al servicio en la nube de hospedaje de escritorio del inquilino. Las direcciones IP públicas se pueden agregar temporalmente cuando sea necesario para tareas de administración, pero se deben eliminar siempre posteriormente.  
  
Para obtener más información, consulta los artículos siguientes:

- [Seguridad y protección](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Procedimientos recomendados de seguridad para IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Protección de Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Consideraciones de diseño

Es importante tener en cuenta las restricciones de los servicios de infraestructura de Microsoft Azure al diseñar un servicio de hospedaje de escritorio multiinquilino. La lista siguiente indica algunos aspectos que el proveedor debería tener en cuenta para conseguir una solución de hospedaje de escritorio funcional y rentable basada en esta arquitectura de referencia.  
  
- Una suscripción de Azure tiene un número máximo de redes virtuales, núcleos de máquinas virtuales y servicios en la nube que se pueden usar. Si un proveedor necesita más recursos, puede que tengas que crear varias suscripciones.
- Un servicio en la nube de Azure tiene un número máximo de máquinas virtuales que se pueden utilizar. Puede que el proveedor tenga que crear varios servicios en la nube para inquilinos más grandes que superan el máximo.  
- Los costos de implementación de Azure se basan, en parte, en el número y tamaño de las máquinas virtuales. El proveedor debe optimizar el número y tamaño de las máquinas virtuales de cada inquilino para proporcionar un entorno de hospedaje de escritorio funcional y altamente seguro al menor costo.  
- Los recursos del equipo físico del centro de datos de Azure se virtualizan mediante Hyper-V. Los hosts de Hyper-V no están configurados en clústeres de hosts, por lo que la disponibilidad de las máquinas virtuales depende de la disponibilidad de los servidores individuales que se usan en la infraestructura de Azure. Para proporcionar una mayor disponibilidad, se pueden crear varias instancias de cada máquina virtual de servicio de rol de un conjunto de disponibilidad para, posteriormente, implementar clústeres de invitados en las máquinas virtuales.  
- En una configuración normal de almacenamiento, un proveedor de servicios tendrá una única cuenta de almacenamiento con varios contenedores (por ejemplo, uno para cada inquilino) y varios discos en cada contenedor. Sin embargo hay un límite de almacenamiento y rendimiento total que se puede conseguir para una única cuenta de almacenamiento. En el caso de proveedores de servicios que admiten grandes cantidades de inquilinos o inquilinos con requisitos elevados de capacidad de almacenamiento o rendimiento, es posible que estos tengan que crear varias cuentas de almacenamiento.  
  
Para obtener más información, consulta los artículos siguientes:

- [Tamaños de los servicios en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Detalles de precios de máquinas virtuales de Microsoft Azure](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Introducción a Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Objetivos de escalabilidad y rendimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy

Azure Active Directory (AD) Application Proxy es un servicio proporcionado en SKU de pago de Azure AD, que permite a los usuarios conectarse a aplicaciones internas a través del servicio propio de proxy inverso de Azure. Esto permite que los puntos de conexión de acceso web y de puerta de enlace de Escritorio remoto estén ocultos dentro de la red virtual, lo que elimina la necesidad de exponerlos a Internet a través de una dirección IP pública. Los proveedores de servicios de hosting pueden usar Azure AD Application Proxy para condensar el número de máquinas virtuales en el entorno del inquilino, a la vez que mantienen una implementación completa. Azure AD Application Proxy habilita también muchas de las ventajas que proporciona Azure AD, como el acceso condicional y la autenticación multifactor.

Para más información, consulta [Introducción a Application Proxy e instalación del conector](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).