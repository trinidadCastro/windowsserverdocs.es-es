---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funciones del sitio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41fe13a5083a855a597a693f8cd707e3e211966a
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81623893"
---
# <a name="site-functions"></a>Funciones del sitio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 usa la información del sitio para muchos propósitos, como la replicación de enrutamiento, la afinidad de cliente, la replicación de volumen del sistema (SYSVOL), los espacios de Sistema de archivos distribuido (DFSN) y la ubicación del servicio.

## <a name="routing-replication"></a>Enrutamiento de la replicación
Active Directory Domain Services (AD DS) utiliza un método de replicación con varios maestros y de almacenamiento y reenvío. Un controlador de dominio comunica los cambios de directorio a un segundo controlador de dominio, que luego se comunica a un tercero, y así sucesivamente, hasta que todos los controladores de dominio hayan recibido el cambio. Para lograr el mejor equilibrio entre la reducción de la latencia de replicación y la reducción del tráfico, los controles de topología de sitio Active Directory la replicación mediante la distinción entre la replicación que se produce dentro de un sitio y la replicación que se produce entre sitios.

Dentro de los sitios, la replicación está optimizada para la velocidad, las actualizaciones de datos desencadenan la replicación y los datos se envían sin la sobrecarga requerida por la compresión de datos. Por el contrario, la replicación entre sitios se comprime para minimizar el costo de transmisión a través de vínculos de red de área extensa (WAN). Cuando se produce la replicación entre sitios, un solo controlador de dominio por dominio en cada sitio recopila y almacena los cambios de directorio y los comunica a un momento programado en un controlador de dominio de otro sitio.

## <a name="client-affinity"></a>Afinidad del cliente
Los controladores de dominio usan información del sitio para informar a los clientes de Active Directory acerca de los controladores de dominio presentes en el sitio más cercano como el cliente. Por ejemplo, Imagine un cliente en el sitio de Seattle que no conoce su afiliación al sitio y se pone en contacto con un controlador de dominio desde el sitio de Atlanta. En función de la dirección IP del cliente, el controlador de dominio de Atlanta determina en qué sitio se encuentra realmente el cliente y envía la información del sitio de vuelta al cliente. El controlador de dominio informa también al cliente de si el controlador de dominio elegido es el más cercano. El cliente almacena en caché la información del sitio proporcionada por el controlador de dominio en Atlanta, consultas para el registro de recursos del servicio específico del sitio (SRV) (un registro de recursos del sistema de nombres de dominio (DNS) que se usa para buscar controladores de dominio para AD DS) y, por tanto, encuentra un controlador de dominio en el mismo sitio.

Al buscar un controlador de dominio en el mismo sitio, el cliente evita las comunicaciones a través de vínculos WAN. Si no se encuentra ningún controlador de dominio en el sitio del cliente, un controlador de dominio que tenga las conexiones de costo más bajo en relación con otros sitios conectados se anuncia a sí mismo (registra un registro de recursos de servicio específico del sitio (SRV) en DNS) en el sitio que no tiene un controlador de dominio. Los controladores de dominio que se publican en DNS son los del sitio más cercano, tal como se define en la topología del sitio. Este proceso garantiza que todos los sitios tengan un controlador de dominio preferido para la autenticación.

Para obtener más información sobre el proceso de búsqueda de un controlador de dominio, vea [Active Directory colección](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10)).

## <a name="sysvol-replication"></a>Replicación de SYSVOL
SYSVOL es una colección de carpetas del sistema de archivos que existe en cada controlador de dominio de un dominio. Las carpetas SYSVOL proporcionan una ubicación de Active Directory predeterminada para los archivos que se deben replicar a través de un dominio, incluidos los objetos de directiva de grupo (GPO), los scripts de inicio y apagado, y los scripts de inicio y cierre de sesión.  Windows Server 2008 puede usar el servicio de replicación de archivos (FRS) o la replicación de Sistema de archivos distribuido (DFSR) para replicar los cambios realizados en las carpetas SYSVOL de un controlador de dominio a otros controladores de dominio. FRS y DFSR replican estos cambios de acuerdo con la programación que se crea durante el diseño de la topología del sitio.

## <a name="dfsn"></a>DFSN
DFSN usa información del sitio para dirigir un cliente al servidor que hospeda los datos solicitados en el sitio. Si DFSN no encuentra una copia de los datos en el mismo sitio que el cliente, DFSN usa la información del sitio en AD DS para determinar qué servidor de archivos tiene los datos compartidos de DFSN más cercanos al cliente.

## <a name="service-location"></a>Ubicación del servicio
Al publicar servicios como servicios de archivos e impresión en AD DS, permite a los clientes de Active Directory localizar el servicio solicitado en el mismo sitio o en el mismo. Los servicios de impresión usan el atributo de ubicación almacenado en AD DS para permitir que los usuarios busquen impresoras por ubicación sin conocer su ubicación precisa. Para obtener más información acerca del diseño y la implementación de servidores de impresión, vea [diseñar e implementar servidores de impresión](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc785842(v=ws.10)).
