---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: "Cuándo se debe crear una granja de servidores de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-farm"></a>Cuándo se debe crear una granja de servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considera la posibilidad de crear una granja de servidores de federación en los servicios de federación de Active Directory \(AD FS\) cuando tienes una implementación de AD FS mayor y desea proporcionar tolerancia a errores, equilibrio load\ o escalabilidad a los servicios de federación de la organización. El act de creación de los servidores de federación de dos o más en la misma red, la configuración de cada uno de ellos para usar el mismo servicio federación y agregar la clave pública de certificados de firma de token\ cada servidor para la administración de AD FS snap\ en crean una granja de servidores de federación.  
  
Puedes crear una granja de servidores de federación o instalar a los servidores de federación adicional a un conjunto existente mediante el Asistente para configuración del servidor de federación de AD FS. Para obtener más información, consulta [cuándo se debe crear un servidor de federación](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Cuando eliges la opción de crear un **nueva granja de servidores de federación** usando el Asistente para configuración del servidor de federación de AD FS, intentará el Asistente Crear un objeto de contenedor \(for sharing certificates\) en Active Directory. Por lo tanto, es importante que primero iniciar sesión en el equipo, dónde estás configurando el rol de servidor de federación, con una cuenta que tenga permisos suficientes en Active Directory para crear este objeto contenedor.  
  
Antes de que se pueden agrupar los servidores de federación como una granja de servidores, primero debe agrupadas para que las solicitudes que llegan a una sola completamente calificado \(FQDN\) se enrutan a los distintos servidores de federación de la granja de servidores de nombre de dominio. Puedes crear el clúster de servidor mediante la implementación de equilibrio de carga de red \(NLB\) dentro de la red corporativa. Esta guía se da por hecho que se ha configurado correctamente NLB para cada uno de los servidores de federación de la batería del clúster.  
  
Para obtener más información sobre cómo configurar un clúster FQDN con tecnología de Microsoft NLB, consulta [especificar los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Los procedimientos recomendados para implementar un conjunto de servidor de federación  
Te recomendamos los siguientes procedimientos recomendados para la implementación de un servidor de federación en un entorno de producción:  
  
-   Si va a implementar varios servidores de federación al mismo tiempo o sabrá que se va a agregar más servidores a la granja de servidores con el tiempo, considera la posibilidad de crear una imagen de servidor de un servidor de federación existente de la batería y luego instalar desde esa imagen cuando se necesita crear rápidamente los servidores de federación adicionales.  
  
    > [!NOTE]  
    > Si decides usar el método de la imagen de servidor para la implementación de los servidores de federación adicionales, no es necesario que completar las tareas en [lista de comprobación: configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) cada vez que desee agregar un nuevo servidor al conjunto.  
  
-   Usar NLB o alguna otra forma de clústeres para asignar una dirección IP para muchos de los equipos de servidor de federación.  
  
-   Reserva una dirección IP estática para cada servidor de federación de la batería y, según la configuración del sistema de nombres de dominio \(DNS\), insertar una exclusión para cada dirección IP en el protocolo de configuración dinámica de Host \(DHCP\). Tecnología de Microsoft NLB requiere que cada servidor que participan en el clúster NLB se asigne una dirección IP estática.  
  
-   Si la base de datos de configuración de AD FS se almacenará en una base de datos SQL, evita la base de datos SQL de varios servidores de federación de edición al mismo tiempo.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configuración de los servidores de federación de un conjunto de  
La siguiente tabla describe las tareas que deben realizarse para que cada servidor de federación pueda participar en un entorno de granja.  
  
|Tarea|Descripción|  
|--------|---------------|  
|Si estás usando SQL Server para almacenar la base de datos de configuración de AD FS|Una granja de servidores de federación consta de dos o más servidores de federación que comparten la misma configuración de AD FS base de datos y certificados de firma de token\. La base de datos de configuración puede almacenarse en cualquier base de datos interna de Windows o en una base de datos de SQL Server. Si tienes previsto almacenar la base de datos de configuración en una base de datos SQL, asegúrate de que la base de datos de configuración sea accesible para que se puede acceder a él por todos los servidores de federación nuevo que participan en el conjunto. **Nota:** para escenarios de granja de servidores, es importante que la base de datos de configuración se encuentra en un equipo que no participar también como un servidor de federación de ese conjunto. Microsoft NLB no admite ninguno de los equipos que participan en un conjunto para comunicarse entre sí. **Nota:** asegurarse de que la identidad de AD FS AppPool en Internet Information Services \(IIS\)\) en cada servidor de federación que participan de la batería tiene acceso de lectura a la base de datos de configuración.|  
|Obtener y compartir certificados|Puedes obtener un único servidor certificado de autenticación de una entidad de certificación públicas \ (CA\), por ejemplo, VeriSign. A continuación, puede configurar el certificado para que todos los servidores de federación comparten la misma parte de clave privada del certificado. Para obtener más información sobre cómo compartir el mismo certificado, consulta [lista de comprobación: configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Nota:** la administración de AD FS en snap\ hace referencia a los certificados de autenticación de servidor para servidores de federación como certificados de comunicación de servicio.<br /><br />Para obtener más información, consulta [requisitos de certificado para los servidores de federación](Certificate-Requirements-for-Federation-Servers.md).|  
|Apunta a la misma instancia de SQL Server|Si la base de datos de configuración de AD FS se almacenará en una base de datos SQL, el servidor de federación nuevo debe apuntar a la misma instancia de SQL Server que sirve otros servidores de federación de la batería para que el nuevo servidor pueda participar en el conjunto.|  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
