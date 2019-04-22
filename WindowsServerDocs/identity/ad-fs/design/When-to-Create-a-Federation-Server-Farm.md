---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Cuándo se debe crear una granja de servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816236"
---
# <a name="when-to-create-a-federation-server-farm"></a>Cuándo se debe crear una granja de servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere la posibilidad de crear una granja de servidores de federación de Active Directory Federation Services \(AD FS\) cuando tiene una implementación mayor de AD FS y desea proporcionar tolerancia a errores, carga\-equilibrio o escalabilidad a su Servicio de federación de su organización. El acto de crear dos o más servidores de federación en la misma red, configurando cada uno de ellos para que use el mismo servicio de federación y agregar el token de la clave pública de cada servidor la\-certificados de firma para el complemento de administración de AD FS\-en crea una granja de servidores de federación.  
  
Puede crear una granja de servidores de federación o instalar a más servidores de federación a una granja existente mediante el Asistente para configuración del servidor de federación de AD FS. Para obtener más información, consulte [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Cuando se elige la opción para crear un **nueva granja de servidores de federación** mediante el Asistente para configuración del servidor de federación de AD FS, el asistente intentará crear un objeto contenedor \(para compartir certificados\) en Active Directory. Por lo tanto, es importante que primero inicies sesión en el equipo donde estás configurando el rol de servidor de federación con una cuenta que tenga permisos suficientes en Active Directory para crear este objeto contenedor.  
  
Antes de que los servidores de federación se pueden agrupar como una granja de servidores, tienen que agrupar para que el nombre de dominio completo de las solicitudes que llegan a una sola totalmente \(FQDN\) se enrutan a los distintos servidores de federación en la granja de servidores. Puede crear el clúster de servidores mediante la implementación de equilibrio de carga de red \(NLB\) dentro de la red corporativa. Esta guía se da por supuesto que NLB se configuró correctamente para cada uno de los servidores de federación en la granja de servidores del clúster.  
  
Para obtener más información sobre cómo configurar un FQDN de clúster con la tecnología NLB de Microsoft, consulte [especificando los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Prácticas recomendadas para implementar una granja de servidores de federación  
Se recomienda las siguientes prácticas recomendadas para implementar un servidor de federación en un entorno de producción:  
  
-   Si va a implementar varios servidores de federación al mismo tiempo o sabe que va a agregar más servidores a la granja de servidores con el tiempo, considere la posibilidad de crear una imagen de servidor de un servidor de federación existente en la granja de servidores y, a continuación, instalación desde esa imagen cuando sea necesario para cr los servidores de federación adicional ear rápidamente.  
  
    > [!NOTE]  
    > Si decide usar el método de la imagen de servidor para implementar más servidores de federación, es necesario completar las tareas de [lista de comprobación: Configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) cada vez que se desea agregar un nuevo servidor a la granja.  
  
-   Usar NLB o algún otro método de agrupación en clústeres para asignar una dirección IP única para muchos equipos de servidores de federación.  
  
-   Reservar una dirección IP estática para cada servidor de federación de la granja de servidores y, según el sistema de nombres de dominio \(DNS\) configuración, insertar una exclusión para cada dirección IP de las direcciones de protocolo de configuración dinámica de Host \(DHCP\). La tecnología de Microsoft NLB requiere que se asigne una dirección IP estática a cada servidor que participa en el clúster NLB.  
  
-   Si la base de datos de configuración de AD FS se almacenarán en una base de datos SQL, evita editar la base de datos SQL de varios servidores de federación al mismo tiempo.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurar los servidores de federación para una granja de servidores  
En la tabla siguiente se describe las tareas que deben completarse para que cada servidor de federación pueda participar en un entorno de granja.  
  
|Tarea|Descripción|  
|--------|---------------|  
|Si estás usando SQL Server para almacenar la base de datos de configuración de AD FS|Una granja de servidores de federación consta de dos o más servidores de federación que comparten la misma base de datos de configuración de AD FS y el token\-certificados de firma. La base de datos de configuración se puede almacenar en la base de datos interna de Windows o en una base de datos de SQL Server. Si va a almacenar la base de datos de configuración en una base de datos SQL, asegúrese de que la base de datos de configuración es accesible, por lo que puede tener acceso a todos los servidores de federación nuevos que participan en la granja de servidores. **Nota:** Para escenarios de granja de servidores, es importante que la base de datos de configuración se encuentra en un equipo que no participe también como un servidor de federación de la granja. Microsoft NLB no permite que participe ninguno de los equipos de una granja de servidores para comunicarse entre sí. **Nota:** Asegúrese de que la identidad de AppPool AD FS en Internet Information Services \(IIS\) \) en cada federación server que participa en la granja de servidores tiene acceso de lectura a la base de datos de configuración.|  
|Obtener y compartir certificados|Puede obtener un único servidor de certificado de autenticación de una entidad de certificación pública \(CA\)— por ejemplo, VeriSign. A continuación, puede configurar el certificado para que todos los servidores de federación comparten la misma parte de clave privada del certificado. Para obtener más información sobre cómo compartir el mismo certificado, consulte [lista de comprobación: Configuración de un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Nota:** El complemento de administración de AD FS\-en hace referencia a certificados de autenticación de servidor para los servidores de federación como certificados de comunicación de servicio.<br /><br />Para obtener más información, consulte [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Selecciona la misma instancia de SQL Server|Si la base de datos de configuración de AD FS se almacenarán en una base de datos SQL, el nuevo servidor de federación debe apuntar a la misma instancia de SQL Server que se usa por otros servidores de federación en la granja de servidores para que el nuevo servidor pueda participar en la granja de servidores.|  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
