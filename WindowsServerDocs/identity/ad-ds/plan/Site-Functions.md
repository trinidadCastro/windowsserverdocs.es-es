---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funciones del sitio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a330a9dae8ab8d3b9de5d1fff0e52060908f520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815816"
---
# <a name="site-functions"></a>Funciones del sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 usa información del sitio para muchos fines, incluida la réplica de enrutamiento, afinidad del cliente, replicación del sistema de volumen (SYSVOL), espacios de nombres del sistema de archivos distribuido (DFSN) y la ubicación del servicio.  
  
## <a name="routing-replication"></a>Réplica de enrutamiento  
Los servicios de dominio de Active Directory (AD DS) usa un método con varios maestros, almacenamiento y reenvío de replicación. Un controlador de dominio comunica los cambios de directorio en un segundo controlador de dominio que se comunica entonces con una tercera y así sucesivamente, hasta que todos los controladores de dominio han recibido el cambio. Para lograr el mejor equilibrio entre lo que reduce la latencia de replicación y reducir el tráfico, topología de sitios controla la replicación de Active Directory diferenciando entre la replicación que se produce dentro de un sitio y que tiene lugar entre sitios.  
  
Dentro de los sitios, la replicación se optimiza para velocidad, la replicación de desencadenador de las actualizaciones de datos y los datos se envían sin la sobrecarga requerida por la compresión de datos. Por el contrario, la replicación entre sitios se comprime para reducir el costo de transmisión a través de vínculos de área extensa (WAN) de la red. Cuando se produce la replicación entre sitios, un controlador de dominio por dominio en cada sitio recopila y almacena los cambios de directorio y comunica a ellos a la hora programada para un controlador de dominio en otro sitio.  
  
## <a name="client-affinity"></a>Afinidad del cliente  
Controladores de dominio usan la información del sitio para informar a los clientes de Active Directory sobre los controladores de dominio presentes en el sitio más cercano que el cliente. Por ejemplo, considere la posibilidad de un cliente en el sitio de Seattle que no conoce su afiliación de un sitio y se pone en contacto con un controlador de dominio desde el sitio de Atlanta. Según la dirección IP del cliente, el controlador de dominio en Atlanta determina qué sitio el cliente realmente desde y envía la información del sitio al cliente. El controlador de dominio también informa al cliente si el controlador de dominio elegido es el más cercano a él. El cliente almacena en caché la información del sitio proporcionada por el controlador de dominio en Atlanta, las consultas para el registro de recursos específicos del sitio de servicios (SRV) (un registro de recursos del sistema de nombres de dominio (DNS) se usa para buscar controladores de dominio de AD DS) y lo que busca un dominio controlador del mismo sitio.  
  
Al encontrar un controlador de dominio en el mismo sitio, el cliente evita las comunicaciones a través de vínculos WAN. Si no hay controladores de dominio se encuentran en el sitio del cliente, un controlador de dominio que tenga las conexiones de costo más bajo en relación con otros sitios conectados se anuncia a sí mismo (registra un registro de recursos específicos del sitio de servicios (SRV) en DNS) en el sitio que no tiene una controlador de dominio. Los controladores de dominio que se publican en DNS son aquellos desde el sitio más cercano, según se define en la topología del sitio. Este proceso garantiza que cada sitio tiene un controlador de dominio preferido para la autenticación.  
  
Para obtener más información sobre el proceso de localizar un controlador de dominio, consulte la recopilación de Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replicación de SYSVOL  
SYSVOL es una colección de carpetas del sistema de archivos que existen en cada controlador de dominio en un dominio. Las carpetas SYSVOL proporcionan una ubicación de Active Directory predeterminado para los archivos que se deben replicar a lo largo de un dominio, incluidos objetos de directiva de grupo (GPO), scripts de inicio y apagado y scripts de inicio de sesión y cierre de sesión.  Windows Server 2008 puede usar el servicio de replicación de archivos (FRS) o la replicación del sistema de archivos distribuido (DFSR) para replicar los cambios realizados en las carpetas SYSVOL de un controlador de dominio a otros controladores de dominio. FRS y DFSR replican estos cambios según la programación que se crea durante el diseño de topología del sitio.  
  
## <a name="dfsn"></a>DFSN  
DFSN usa información del sitio para dirigir a un cliente en el servidor que hospeda los datos solicitados dentro del sitio. Si DFSN no encuentra una copia de los datos dentro del mismo sitio que el cliente, DFSN usa la información del sitio en AD DS para determinar qué servidor de archivos que tiene DFSN datos compartidos es más cercana al cliente.  
  
## <a name="service-location"></a>Ubicación de servicio  
Mediante la publicación de servicios como archivos y servicios de impresión en AD DS, permite a los clientes de Active Directory localizar el servicio solicitado en la misma o más próximo al sitio. Servicios de impresión utilice el atributo de ubicación almacenado en AD DS para que los usuarios buscar impresoras por ubicación sin conocer su ubicación exacta. Para obtener más información sobre cómo diseñar e implementar servidores de impresión, vea Diseñar e implementar servidores de impresión ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


