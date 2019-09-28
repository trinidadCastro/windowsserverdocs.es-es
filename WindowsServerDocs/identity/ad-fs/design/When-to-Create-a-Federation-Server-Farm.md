---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Cuándo se debe crear una granja de servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fcfc7d640d3688bf0e23557af9bd56082418ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358943"
---
# <a name="when-to-create-a-federation-server-farm"></a>Cuándo se debe crear una granja de servidores de federación

Considere la posibilidad de crear una granja de servidores de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 si tiene una implementación de AD FS más grande y desea proporcionar tolerancia a errores, cargar @ no__t-2balancing o escalabilidad a la Federación de su organización. Servicio. La acción de crear dos o más servidores de Federación en la misma red, configurando cada uno de ellos para que use el mismo Servicio de federación y agregando la clave pública de los certificados de token @ no__t-0signing de cada servidor al complemento de administración de AD FS @ no__t-1in crea un granja de servidores de Federación.  
  
Puede crear una granja de servidores de Federación o instalar más servidores de Federación en una granja existente mediante el Asistente para configuración de servidor de Federación de AD FS. Para obtener más información, consulte [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Cuando eliges la opción de crear una **nueva granja de servidores de Federación** mediante el Asistente para configuración de servidor de federación de AD FS, el asistente intentará crear un objeto contenedor \(Para compartir certificados @ no__t-2 en Active Directory. Por lo tanto, es importante que primero inicies sesión en el equipo donde estás configurando el rol de servidor de federación con una cuenta que tenga permisos suficientes en Active Directory para crear este objeto contenedor.  
  
Antes de que los servidores de Federación puedan agruparse como una granja, primero deben agruparse para que las solicitudes que llegan a un nombre de dominio completo único \(FQDN @ no__t-1 se enruten a los distintos servidores de Federación de la granja de servidores. Puede crear el clúster de servidores mediante la implementación del equilibrio de carga de red \(NLB @ no__t-1 dentro de la red corporativa. En esta guía se da por supuesto que se ha configurado correctamente NLB para agrupar cada uno de los servidores de Federación de la granja.  
  
Para obtener más información sobre cómo configurar un FQDN de clúster con la tecnología NLB de Microsoft, consulte [especificación de los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Prácticas recomendadas para implementar una granja de servidores de federación  
Recomendamos las siguientes prácticas recomendadas para implementar un servidor de Federación en un entorno de producción:  
  
-   Si va a implementar varios servidores de Federación al mismo tiempo o sabe que va a agregar más servidores a la granja con el tiempo, considere la posibilidad de crear una imagen de servidor de un servidor de Federación existente en la granja y, a continuación, instalar desde esa imagen cuando necesite CR EAR más servidores de Federación rápidamente.  
  
    > [!NOTE]  
    > Si decide usar el método de imagen de servidor para implementar servidores de Federación adicionales, no es necesario completar las tareas en [Checklist: La configuración de un servidor de Federación @ no__t-0 cada vez que desea agregar un nuevo servidor a la granja.  
  
-   Use NLB o algún otro tipo de agrupación en clústeres para asignar una sola dirección IP para muchos equipos del servidor de Federación.  
  
-   Reserve una dirección IP estática para cada servidor de Federación de la granja y, en función de la configuración del sistema de nombres de dominio \(DNS @ no__t-1, inserte una exclusión para cada dirección IP en el protocolo de configuración dinámica de host \(DHCP @ no__t-3. La tecnología de Microsoft NLB requiere que se asigne una dirección IP estática a cada servidor que participa en el clúster NLB.  
  
-   Si la base de datos de configuración de AD FS se almacenará en una base de datos SQL, evite editar la base de datos SQL desde varios servidores de Federación al mismo tiempo.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurar los servidores de federación para una granja de servidores  
En la tabla siguiente se describen las tareas que deben completarse para que cada servidor de Federación pueda participar en un entorno de granja.  
  
|Tarea|Descripción|  
|--------|---------------|  
|Si estás usando SQL Server para almacenar la base de datos de configuración de AD FS|Una granja de servidores de Federación consta de dos o más servidores de Federación que comparten la misma base de datos de configuración de AD FS y los certificados de token @ no__t-0signing. La base de datos de configuración se puede almacenar en la base de datos interna de Windows o en una base de datos de SQL Server. Si tiene previsto almacenar la base de datos de configuración en una base de datos SQL, asegúrese de que se pueda acceder a la base de datos de configuración para que todos los servidores de Federación nuevos que participan en la granja puedan tener acceso a ella. **Nota:** En escenarios de granja de servidores, es importante que la base de datos de configuración se encuentre en un equipo que no participe también como servidor de Federación en esa granja. Microsoft NLB no permite que participe ninguno de los equipos de una granja de servidores para comunicarse entre sí. **Nota:** Asegúrese de que la identidad del AD FS AppPool en Internet Information Services \(IIS @ no__t-1 @ no__t-2 en cada servidor de Federación que participa en la granja tenga acceso de lectura a la base de datos de configuración.|  
|Obtener y compartir certificados|Puede obtener un único certificado de autenticación de servidor de una entidad de certificación pública \(CA @ no__t-1, por ejemplo, VeriSign. Después, puede configurar el certificado para que todos los servidores de Federación compartan la misma parte de la clave privada del certificado. Para obtener más información acerca de cómo compartir el mismo certificado, vea [Checklist: Configuración de un servidor de Federación @ no__t-0. **Nota:** El complemento de administración de AD FS @ no__t-0in hace referencia a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.<br /><br />Para obtener más información, consulte [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Selecciona la misma instancia de SQL Server|Si la base de datos de configuración de AD FS se almacenará en una base de datos SQL, el nuevo servidor de Federación debe apuntar a la misma instancia de SQL Server que usan otros servidores de Federación de la granja para que el nuevo servidor pueda participar en la granja.|  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
