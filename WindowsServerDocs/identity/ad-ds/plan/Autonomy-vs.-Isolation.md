---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomía frente a Aislamiento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 765a25d3d1ffdb4df473e1fb5bb65e532934aca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867576"
---
# <a name="autonomy-vs-isolation"></a>Autonomía frente a Aislamiento

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede diseñar la estructura lógica de Active Directory para lograr una de las siguientes:  
  
-   **Autonomía**. Implica control independiente, pero no es algo exclusivo de un recurso. Al obtener autonomía, los administradores tienen autoridad para administrar los recursos de forma independiente; Sin embargo, los administradores con mayor autoridad existen que también tiene control sobre los recursos y puede evitarle control si es necesario. Puede diseñar la estructura lógica de Active Directory para lograr los siguientes tipos de autonomía:  
  
    -   **Autonomía del servicio**. Este tipo de autonomía implica control sobre todo o parte de la administración de servicio.  
  
    -   **Autonomía datos**. Este tipo de autonomía implica control sobre todo o parte de los datos almacenados en el directorio o en equipos pertenecientes a unirse al directorio.  
  
-   **Aislamiento**. Implica el control exclusivo e independiente de un recurso. Al conseguir el aislamiento, los administradores tienen autoridad para administrar un recurso de forma independiente y ningún otro administrador puede evitarle control del recurso. Puede diseñar la estructura lógica de Active Directory para lograr los siguientes tipos de aislamiento:  
  
    -   **Aislamiento del servicio**. Evita que los administradores (que no sean los administradores que son designados específicamente para controlar la administración de servicios) controlar o interfieran con la administración de servicios.  
  
    -   **Aislamiento de los datos**. Evita que los administradores (que no sean los administradores que son designados específicamente a los datos de control o vista) controlar o ver un subconjunto de datos en el directorio o en equipos pertenecientes a unirse al directorio.  
  
Administradores que necesiten autonomía solo aceptan que otros administradores que tienen autoridad administrativa igual o mayor tienen igual o mayor control sobre la administración de datos o servicio. Los administradores que necesitan aislamiento tienen control exclusivo sobre la administración de datos o servicio. Crear un diseño para lograr la autonomía es generalmente menos costoso que la creación de un diseño para conseguir el aislamiento.  
  
En los servicios de dominio de Active Directory (AD DS), los administradores pueden delegar la administración de servicios y administración de datos para lograr la autonomía o aislamiento entre las organizaciones. La combinación de administración de servicios, los requisitos de aislamiento, autonomía y administración de datos de una organización afectan a los contenedores de Active Directory que se usan para delegar la administración.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisitos de aislamiento y autonomía  
El número de bosques que necesita para implementar se basa en los requisitos de aislamiento y autonomía de cada grupo dentro de su organización. Para identificar los requisitos de diseño de bosque, debe identificar los requisitos de autonomía y aislamiento para todos los grupos de su organización. En concreto, debe identificar la necesidad de aislamiento de los datos, autonomía de datos, el aislamiento del servicio y la autonomía del servicio. También debe identificar las áreas de conectividad limitada en su organización.  
  
### <a name="data-isolation"></a>Aislamiento de los datos  
Aislamiento de los datos implica el control exclusivo sobre datos por el grupo o la organización que posee los datos. Es importante tener en cuenta que los administradores de servicios tienen la capacidad de tomar el control de un recurso de los administradores de datos. Y los administradores de datos no tienen la capacidad para evitar que los administradores de servicios de acceso a los recursos que controlan. Por lo tanto, no se puede conseguir el aislamiento de datos al otro grupo dentro de la organización es responsable de la administración del servicio. Si un grupo requiere aislamiento de los datos, ese grupo también debe asumir la responsabilidad para la administración de servicio.  
  
Porque los datos almacenan en AD DS y en los equipos unidos a AD DS no se pueden aislados a los administradores de servicio, es la única forma de un grupo dentro de una organización y así conseguir aislamiento completo de datos crear un bosque independiente para los datos. Las organizaciones que son importantes las consecuencias de un ataque por software malintencionado o por un administrador de servicios forzada puede elegir crear un bosque independiente para lograr el aislamiento de los datos. Los requisitos legales normalmente crean una necesidad para este tipo de aislamiento de los datos. Por ejemplo:  
  
-   Una institución financiera está obligada por ley a limitar el acceso a datos que pertenecen a los clientes de una jurisdicción a usuarios, equipos y los administradores ubicados en una jurisdicción determinada. Aunque la institución confía en los administradores de servicios que funcionan fuera del área protegido, si se infringe la limitación de acceso, la entidad ya no será capaz de hacer negocios en una jurisdicción. Por lo tanto, la institución financiera debe aislar los datos de los administradores de servicio fuera de la jurisdicción. Tenga en cuenta que el cifrado no siempre es una alternativa a esta solución. Cifrado no puede proteger los datos de los administradores de servicios.  
  
-   Para limitar el acceso a datos del proyecto a un conjunto de usuarios, un contratista defensa está obligado por ley. Aunque el contratista confía en administradores de servicios que controlan los sistemas de equipo relacionados con otros proyectos, una infracción de esta limitación de acceso hará que los contratistas a perder negocio.  
  
    > [!NOTE]  
    > Si tiene un requisito de aislamiento de datos, debe decidir si es necesario aislar los datos de los administradores de servicios o de los administradores de datos y los usuarios normales. Si el requisito de aislamiento se basa en el aislamiento de usuarios normales y los administradores de datos, puede usar listas de control de acceso (ACL) para aislar los datos. Para los fines de este proceso de diseño, aislada con respecto a los administradores de datos y los usuarios normales no se considera un requisito de aislamiento de datos.  
  
### <a name="data-autonomy"></a>Autonomía de datos  
Autonomía de datos implica la capacidad de una organización o grupo para administrar sus propios datos, incluidos decisiones administrativas sobre los datos y realizar las tareas administrativas necesarias sin necesidad de aprobación desde otra entidad.  
  
Autonomía de datos no impide que los administradores de servicios en el bosque de acceso a los datos. Por ejemplo, un grupo de investigación de una organización grande podría desea ser capaz de administrar sus datos específicos del proyecto por sí mismos, pero no es necesario proteger los datos de otros administradores en el bosque.  
  
### <a name="service-isolation"></a>Aislamiento del servicio  
Aislamiento del servicio implica el control exclusivo de la infraestructura de Active Directory. Grupos que requieran el aislamiento del servicio requieren que ningún administrador fuera del grupo puede interferir con el funcionamiento del servicio de directorio.  
  
Los requisitos operativos o legales normalmente crean una necesidad de aislamiento del servicio. Por ejemplo:  
  
-   Una empresa de fabricación tiene una aplicación crítica que controla los equipos en la fábrica. Interrupciones del servicio en otras partes de la red de la organización no tenga permiso para interferir con el funcionamiento de la fábrica.  
  
-   Una empresa de hospedaje proporciona servicio a varios clientes. Cada cliente requiere aislamiento del servicio para que ninguna interrupción del servicio que afecta a un cliente no afecta a los demás clientes.  
  
### <a name="service-autonomy"></a>Autonomía del servicio  
La autonomía del servicio implica la capacidad para administrar la infraestructura sin un requisito para el control exclusivo; Por ejemplo, cuando un grupo desea realizar cambios en la infraestructura (por ejemplo, agregar o quitar dominios, modificar el espacio de nombres del sistema de nombres de dominio (DNS) o la modificación del esquema) sin la aprobación del propietario del bosque.  
  
La autonomía del servicio podría ser necesaria dentro de una organización para un grupo que desea poder controlar el nivel de servicio de AD DS (al agregar y quitar controladores de dominio, según sea necesario) o para un grupo que necesita para poder instalar aplicaciones habilitadas para directorios que requiere extensiones de esquema.  
  
## <a name="limited-connectivity"></a>Conectividad limitada  
Si un grupo dentro de su organización es propietaria de las redes que están separadas por los dispositivos que restringen o limitan la conectividad entre redes (como los firewalls y dispositivos de traducción de direcciones de red (NAT)), esto puede afectar el diseño de bosque. Al identificar los requisitos de diseño de bosque, asegúrese de tener en cuenta las ubicaciones donde haya limitado la conectividad de red. Esta información es necesaria para que pueda tomar decisiones sobre el diseño de bosque.  
  


