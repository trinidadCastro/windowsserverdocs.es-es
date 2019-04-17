---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: "Creación de un diseño de vínculo del sitio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>Creación de un diseño de vínculo del sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crear un diseño de vínculo del sitio para conectar tus sitios con vínculos a sitios. Vínculos a sitios reflejan la conectividad entre los sitios y el método que se usa para transferir el tráfico de replicación. Debes conectar sitios con vínculos a sitios para que los controladores de dominio en cada sitio pueden replicar los cambios de Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Conexión de sitios con vínculos a sitios  
Para conectar sitios con vínculos a sitios, identificar los sitios de miembro que desea conectar con el vínculo al sitio, crear un objeto de enlace de sitio en el contenedor de transportes entre sitios correspondiente y, a continuación, el nombre del vínculo a sitios. Después de crear el vínculo al sitio, puede continuar para establecer el sitio de propiedades de enlace.  
  
Al crear vínculos a sitios, asegúrate de que todos los sitios se incluyen en un vínculo de sitio. Además, asegúrate de que todos los sitios estén conectados entre sí a través de otros vínculos a sitios para que los cambios pueden replicar desde los controladores de dominio en cualquier sitio en todos los demás sitios. Si no lo haces esto, se genera un mensaje de error en el registro de servicio de directorio en el Visor de eventos que indica que la topología de sitio no está conectada.  
  
Siempre que agregar sitios a un vínculo de sitio recién creado, determinar si el sitio se agrega es un miembro de vínculos a otros sitios y cambiar la pertenencia al vínculo de sitio del sitio si es necesario. Por ejemplo, si realizas un sitio de un miembro del Default-First-Site-Link cuando se crea inicialmente el sitio, asegúrate de quitar el sitio del Default-First-Site-Link después de agregar el sitio a un nuevo vínculo del sitio. Si no quitas el sitio desde el Default-First-Site-Link, el Comprobador de coherencia de la información (KCC) se tomar decisiones de enrutamiento basadas en la pertenencia de los vínculos a sitios, lo que puede ocasionar enrutamiento incorrecto.  
  
Para identificar los sitios de miembro que desea conectarse con un vínculo del sitio, usa la lista de ubicaciones y ubicaciones vinculadas que anotó en la hoja de cálculo de (DSSTOPO_1.doc) "Geográfica ubicaciones y vínculos de comunicación". Si varios sitios tienen el mismo conectividad y disponibilidad entre sí, se puede conectar con el mismo vínculo del sitio.  
  
El contenedor transportes entre sitios proporciona los medios para asignar los vínculos a sitios con el transporte que usa el vínculo. Cuando se crea un objeto de enlace de sitio, se crea en el contenedor IP, que se asocia el vínculo del sitio con la llamada a procedimiento remoto (RPC) por medio de transporte IP, o el contenedor de protocolo de transferencia de correo (SMTP), que asocia el vínculo del sitio con el transporte SMTP.  
  
> [!NOTE]  
> Replicación SMTP no se admitirán en futuras versiones de los servicios de dominio de Active Directory (AD DS); por lo tanto, no se recomienda crear objetos de vínculos a sitios en el contenedor SMTP.  
  
Cuando se crea un objeto de enlace de sitio en el contenedor transportes entre sitios respectivo, AD DS usa RPC sobre IP para transferir entre sitios y dentro de un sitio de replicación entre controladores de dominio. Para proteger datos mientras están en tránsito, RPC a través de replicación IP usa tanto la Kerberos autenticación protocolo y cifrado de datos.  
  
Cuando una conexión directa de IP no está disponible, puedes configurar la replicación entre sitios usar SMTP. Sin embargo, la funcionalidad de replicación SMTP es limitada y requiere una entidad de certificación (CA) de empresa. SMTP solo puede duplicar la configuración, esquema y las particiones de directorio de aplicación y no admite la replicación de particiones de directorio de dominio.  
  
Vínculos a sitios de nombre, usa un esquema de nomenclatura coherente, como name_of_site1 name_of_site2. Registra la lista de sitios, los sitios vinculados y los nombres de los vínculos a sitios conectando estos sitios en una hoja de cálculo. Para que una hoja de cálculo que le ayudarán a grabar los nombres de sitio y vínculo sitio asociado, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, y Abre (DSSTOPO_5.doc) "Sitios y vínculos a sitios asociados".  
  
## <a name="in-this-guide"></a>En esta guía  
[Establecer propiedades de enlace de sitio](Setting-Site-Link-Properties.md)  
  


