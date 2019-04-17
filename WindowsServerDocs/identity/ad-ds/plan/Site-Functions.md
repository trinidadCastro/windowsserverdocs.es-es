---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funciones de sitio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f40122d84185c69fa19f5158c2b1c370ec1bfe82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="site-functions"></a>Funciones de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 usa información del sitio para varios propósitos, como la replicación de enrutamiento, afinidad del cliente, replicación del sistema (SYSVOL) de volumen, los espacios de nombres del sistema de archivos distribuido (DFSN) y la ubicación del servicio.  
  
## <a name="routing-replication"></a>Enrutamiento de replicación  
Los servicios de dominio de Active Directory (AD DS) usa un método con varios maestros, almacenamiento y reenvío de replicación. Un controlador de dominio comunica a los cambios de directorio a un segundo controlador de dominio, que, a continuación, se comunica a un tercero y así sucesivamente, hasta que todos los controladores de dominio hayan recibido el cambio. Para lograr el mejor equilibrio entre lo que reduce la latencia de replicación y tráfico, la topología de sitio controla la replicación de Active Directory distinguir entre la duplicación que se produce dentro de un sitio y que se produce entre sitios.  
  
Dentro de los sitios, replicación está optimizada para la replicación de desencadenador de speeddata actualizaciones y los datos se envían sin la sobrecarga que lo exija la compresión de datos. Por el contrario, la replicación entre sitios está comprimida para reducir el costo de la transmisión a través de vínculos de área extensa (WAN) de la red. Cuando se produce la replicación entre sitios, un controlador de dominio por dominio en cada sitio recopila y almacena los cambios de directorio y comunica a ellos en un horario programado a un controlador de dominio en otro sitio.  
  
## <a name="client-affinity"></a>Afinidad del cliente  
Controladores de dominio usan información del sitio para informar a los clientes de Active Directory sobre controladores de dominio presentes en el sitio más cercano que el cliente. Por ejemplo, considera la posibilidad de un cliente en el sitio de Seattle que no se conoce su afiliación de sitio y los contactos de un controlador de dominio desde el sitio de Atlanta. En función de la dirección IP del cliente, el controlador de dominio en Atlanta determina el sitio que el cliente es realmente de y envía la información del sitio al cliente. El controlador de dominio también informa al cliente si el controlador de dominio elegida es el más parecido a ella. El cliente almacena en caché la información del sitio proporcionada por el controlador de dominio en Atlanta, consulta el registro de recursos específicos del sitio de servicio (SRV) (sistema de nombres de dominio (DNS) registro de recursos usado para buscar controladores de dominio de AD DS) y lo que encuentra un controlador de dominio dentro del mismo sitio.  
  
Buscando un controlador de dominio en el mismo sitio, el cliente evita comunicarse a través de los vínculos WAN. Si no hay controladores de dominio se encuentran en el sitio del cliente, un controlador de dominio que tenga las conexiones de costo más bajas en relación con otros sitios conectados anuncie (registra un registro de recursos específicos del sitio de servicio (SRV) en DNS) en el sitio que no tiene un controlador de dominio. Los controladores de dominio que se publican en DNS son los desde el sitio más cercano, tal como define la topología de sitio. Este proceso garantiza que cada sitio tiene un controlador de dominio preferido para la autenticación.  
  
Para obtener más información sobre el proceso de localización de un controlador de dominio, consulta la recopilación de Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replicación de SYSVOL  
SYSVOL es una colección de carpetas del sistema de archivos que existen en cada controlador de dominio en un dominio. Las carpetas SYSVOL proporcionan una ubicación de Active Directory predeterminada para los archivos que se debe replicar en todo un dominio, incluidos los objetos de directiva de grupo (GPO), scripts de inicio y cierre y los scripts de inicio de sesión y cierre de sesión.  Windows Server 2008 puede utilizar el servicio de replicación de archivos (FRS) o la replicación del sistema de archivos distribuido (DFSR) para replicar los cambios realizados en las carpetas SYSVOL de un controlador de dominio a otros controladores de dominio. FRS y DFSR replican estos cambios según la programación que creas durante el diseño de la topología de sitio.  
  
## <a name="dfsn"></a>DFSN  
DFSN usa información del sitio para dirigir a un cliente al servidor que hospeda los datos solicitados dentro del sitio. Si DFSN no encuentra una copia de los datos dentro del mismo sitio que el cliente, DFSN usa la información del sitio en AD DS para determinar qué servidor de archivos que tiene DFSN datos compartidos es más cercana al cliente.  
  
## <a name="service-location"></a>Ubicación del servicio  
Mediante la publicación de servicios como archivos y servicios de impresión en AD DS, puedes permitir que los clientes de Active Directory localizar el servicio solicitado dentro del mismo o más cercano de sitio. Servicios de impresión, usan el atributo de ubicación almacenado en AD DS para que los usuarios puedan buscar impresoras por ubicación sin conocer su ubicación precisa. Para obtener más información sobre cómo diseñar e implementar los servidores de impresión, consulta el diseño y la implementación de servidores de impresión ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


