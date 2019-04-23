---
title: Servicios de Azure y consideraciones para el hospedaje de escritorio
description: Obtenga información acerca de las consideraciones únicas en Azure con una solución de hospedaje de escritorio remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: 37210a5d75399309c53364f5b8ee9e06d26d6f32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849806"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Servicios de Azure y consideraciones para el hospedaje de escritorio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Las secciones siguientes describen los servicios de infraestructura de Azure.
  
## <a name="azure-portal"></a>Portal de Azure

Después de que el proveedor crea una suscripción de Azure, el portal de Azure puede usarse para crear manualmente el entorno de cada inquilino. Este proceso también se puede automatizar mediante scripts de PowerShell.  

Para obtener más información, visite la [Microsoft Azure](https://www.azure.microsoft.com) sitio Web.
  
## <a name="azure-load-balancer"></a>Equilibrador de carga de Azure

Componentes del inquilino se ejecutan en máquinas virtuales que se comunican entre sí en una red aislada. Durante el proceso de implementación externamente puede tener acceso a estas máquinas virtuales, a través del equilibrador de carga de Azure mediante los puntos de conexión de protocolo de escritorio remoto o un punto de conexión de PowerShell remoto. Una vez completada una implementación, normalmente se eliminarán estos puntos de conexión para reducir el área expuesta a ataques. Los puntos de conexión solo será los puntos de conexión HTTPS y UDP creados para la máquina virtual ejecuta los componentes de puerta de enlace de escritorio remoto y Web de escritorio remoto. Esto permite a los clientes en internet para conectarse a las sesiones que se ejecutan en el servicio de hospedaje de escritorio del inquilino. Si un usuario abre una aplicación que se conecta a internet, como un explorador web, las conexiones se pasarán a través del equilibrador de carga de Azure.  
  
Para obtener más información, consulte [¿qué es Azure Load Balancer?](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>Consideraciones de seguridad

Esta guía arquitectura de referencia de hospedaje de escritorio de Azure está diseñada para proporcionar un entorno altamente seguro y aislado para cada inquilino. Seguridad del sistema también depende de las medidas de seguridad realizadas por el proveedor durante la implementación y el funcionamiento del servicio hospedado. En la lista siguiente se describe algunas consideraciones que debe realizar el proveedor para mantener su solución de hospedaje de escritorio basada en esta arquitectura de referencia segura.

- Todas las contraseñas administrativas deben ser seguras, lo ideal es que al azar generado, cambia con frecuencia y guardado en una ubicación central segura solo es accesible a una selección pocos administradores de proveedor.  
- Al replicar el entorno de inquilinos para inquilinos nuevos, evite el uso de las contraseñas administrativas mismas o débiles.
- La dirección URL del sitio acceso Web de escritorio remoto, el nombre y los certificados deben ser único y reconocible para cada inquilino para evitar ataques de suplantación de identidad.  
- Durante el funcionamiento normal del servicio de hospedaje de escritorio, se deben eliminar todas las direcciones IP públicas para todas las máquinas virtuales, excepto la máquina virtual de Web de escritorio remoto y puerta de enlace de escritorio remoto que permite a los usuarios conectarse de forma segura al servicio en la nube de hospedaje escritorio del inquilino. Las direcciones IP públicas se pueden agregar temporalmente cuando sea necesario para las tareas de administración, pero siempre deberían eliminarse después.  
  
Para obtener más información, consulta los artículos siguientes:

- [Seguridad y protección](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Prácticas recomendadas de seguridad para IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Secure Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Consideraciones de diseño

Es importante tener en cuenta las restricciones de los servicios de infraestructura de Microsoft Azure al diseñar un servicio de hospedaje de escritorio para varios inquilinos. En la lista siguiente describe las consideraciones que debe realizar el proveedor para lograr una solución de hospedaje escritorio funcional y rentable basada en esta arquitectura de referencia.  
  
- Una suscripción de Azure tiene un número máximo de redes virtuales, núcleos de máquinas virtuales y servicios en la nube que se puede usar. Si un proveedor de necesita más recursos que esto, necesitan crear varias suscripciones.
- Un servicio en la nube de Azure tiene un número máximo de máquinas virtuales que se puede usar. El proveedor que deba crear varios servicios en la nube para los inquilinos más grandes que superan el máximo.  
- Los costos de implementación de Azure se basan parcialmente en el número y tamaño de las máquinas virtuales. El proveedor debería optimizar el número y tamaño de las máquinas virtuales para cada inquilino para proporcionar una funcional y entorno de hospedaje de escritorio con el costo más bajo de alta seguridad.  
- Los recursos de equipo físico en el centro de datos de Azure están virtualizados mediante Hyper-V. Hosts de Hyper-V no están configurados en clústeres de hosts, por lo que la disponibilidad de las máquinas virtuales depende de la disponibilidad de los servidores individuales usados en la infraestructura de Azure. Para proporcionar una mayor disponibilidad, pueden crearse varias instancias de cada servicio de rol de la máquina virtual en un conjunto de disponibilidad, a continuación, se pueden implementar clústeres invitados dentro de las máquinas virtuales.  
- En una configuración típica de almacenamiento, un proveedor de servicios tendrá una única cuenta de almacenamiento con varios contenedores (por ejemplo, uno para cada inquilino) y varios discos en cada contenedor. Sin embargo, hay un límite para el almacenamiento total y el rendimiento que se puede conseguir para una única cuenta de almacenamiento. Para los proveedores de servicios que admiten grandes cantidades de inquilinos o inquilinos con requisitos de almacenamiento alto rendimiento o capacidad, el proveedor de servicios que deba crear varias cuentas de almacenamiento.  
  
Para obtener más información, consulta los artículos siguientes:

- [Tamaños de Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Detalles de precios de Microsoft Azure virtual machine](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Introducción a Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Objetivos de escalabilidad y rendimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Proxy de aplicación de Azure Active Directory

Proxy de aplicación de Azure Active Directory (AD) es un servicio proporcionado en el pago las SKU de Azure AD que permiten a los usuarios conectarse a las aplicaciones internas a través del servicio de proxy inverso de Azure. Esto permite que los puntos de conexión Web a Escritorio remoto y puerta de enlace de escritorio remoto esté oculto dentro de la red virtual, sin necesidad de estar expuesto a internet mediante una dirección IP pública. Los proveedores de hospedaje pueden usar a Azure AD Application Proxy para condensar el número de máquinas virtuales en el entorno del inquilino manteniendo una implementación completa. Proxy de aplicación de Azure AD también permite que muchas de las ventajas que proporciona Azure AD, como acceso condicional y Multi-factor authentication.

Para obtener más información, consulte [empezar a trabajar con el Proxy de aplicación e instalar el conector](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).