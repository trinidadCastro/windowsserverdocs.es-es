---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: "Autonomía frente aislamiento"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19613b209399c61747af6a2d1fbe243dbcb92225
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="autonomy-vs-isolation"></a>Autonomía frente aislamiento

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes diseñar la estructura lógica de Active Directory para lograr una de las siguientes:  
  
-   **Autonomía**. Implica un control independiente, pero no exclusivo de un recurso. Cuando lograr autonomía, los administradores tienen la autoridad para administrar recursos de forma independiente; Sin embargo, los administradores de TI mayor autoridad existen que también tiene control sobre esos recursos y puede tomar el control inmediatamente si es necesario. Puedes diseñar la estructura lógica de Active Directory para lograr los siguientes tipos de autonomía:  
  
    -   **Servicio autonomía**. Este tipo de autonomía implica control sobre todo o parte de la administración de servicios.  
  
    -   **Autonomía de datos**. Este tipo de autonomía implica control sobre todo o parte de los datos almacenados en el directorio o en los equipos miembro Unidos al directorio.  
  
-   **Aislamiento**. Implica un control independiente y exclusivo de un recurso. Cuando lograr aislamiento, los administradores tienen la autoridad para administrar un recurso de forma independiente y ningún otro administrador puede controlar lejos del recurso. Puedes diseñar la estructura lógica de Active Directory para lograr los siguientes tipos de aislamiento:  
  
    -   **El aislamiento de servicio**. Impide que los administradores (que no sean los administradores que están designados específicamente para controlar la administración de servicios) controlen o interfieran con la administración de servicios.  
  
    -   **El aislamiento de datos**. Impide que los administradores (que no sean los administradores que están designados específicamente para los datos de control o vista) controlar o ver un subconjunto de datos en el directorio o en los equipos miembro Unidos al directorio.  
  
Los administradores que requieren autonomía solo aceptan que otros administradores que tienen autoridad administrativa igual o mayor tienen igual o mayor control sobre la administración de datos o servicio. Los administradores que necesiten aislamiento tienen exclusivo control sobre la administración de datos o servicio. Crear un diseño para lograr autonomía es por lo general, menos costoso que crear un diseño para lograr el aislamiento.  
  
En los servicios de dominio de Active Directory (AD DS), los administradores pueden delegar la administración del servicio y administración de datos para lograr la autonomía o aislamiento entre las organizaciones. La combinación de administración de servicios, los requisitos de administración, autonomía y aislamiento de datos de una organización afectan a los contenedores de Active Directory que se usan para delegar la administración.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisitos de aislamiento y autonomía  
El número de bosques que necesitas para implementar se basa en los requisitos de autonomía y aislamiento de cada grupo dentro de la organización. Para identificar los requisitos de diseño de bosque, debe identificar los requisitos de aislamiento y autonomía para todos los grupos en la organización. Específicamente, debe identificar la necesidad de aislamiento de datos, autonomía de datos, aislamiento del servicio y autonomía del servicio. También debe identificar las áreas de conectividad limitada en la organización.  
  
### <a name="data-isolation"></a>Aislamiento de datos  
El aislamiento de datos implica control exclusivo datos por el grupo o la organización que posee los datos. Es importante tener en cuenta que los administradores de servicio tienen la capacidad de tomar el control de un recurso de los administradores de datos. Y los administradores de datos no tienen la capacidad para impedir que los administradores del servicio de acceso a los recursos que controlan. Por lo tanto, no se puede lograr el aislamiento de datos cuando otro grupo dentro de la organización es responsable de administración del servicio. Si un grupo requiere el aislamiento de datos, ese grupo también debe asumir la responsabilidad de administración del servicio.  
  
Dado que los datos se almacenan en AD DS y en equipos unidos a AD DS no pueden ser aislados de los administradores de servicio, el único modo de un grupo dentro de una organización para lograr el aislamiento de datos completo consiste en crear un bosque independiente para que los datos. Las organizaciones que son importantes las consecuencias de un ataque por software malintencionado o por un administrador de servicios convertidos puede crear un bosque independiente para lograr el aislamiento de datos. Los requisitos legales normalmente crean una necesidad para este tipo de aislamiento de datos. Por ejemplo:  
  
-   Una entidad financiera se exija la ley a limitar el acceso a datos que pertenecen a los clientes en una determinada jurisdicción a los usuarios, equipos y administradores ubicados en esa jurisdicción. Aunque la institución confía en los administradores de servicios que funcionan fuera del área protegido, si se infringe la limitación de acceso, la institución ya no podrán hacer negocios en esa jurisdicción. Por lo tanto, la institución financiera debe aislar los datos de los administradores de servicio fuera de esa jurisdicción. Ten en cuenta que el cifrado no siempre es una alternativa a esta solución. Cifrado no puede proteger los datos de los administradores de servicio.  
  
-   Legislación requiere un contratista defensa para limitar el acceso a los datos de project a un conjunto específico de usuarios. Aunque el contratista confía en los administradores de servicio que controlan los sistemas informáticos relacionados con los otros proyectos, una infracción de esta limitación acceso hará que el contratista perder empresarial.  
  
    > [!NOTE]  
    > Si tienes un requisito de aislamiento de datos, debes decidir si debes aislar los datos de los administradores de servicio o de los administradores de datos y los usuarios normales. Si el requisito de aislamiento se basa en el aislamiento de los administradores de datos y los usuarios normales, puedes usar listas de control de acceso (ACL) para aislar los datos. Para los fines de este proceso de diseño, el aislamiento de los administradores de datos y los usuarios normales no se considera un requisito de aislamiento de datos.  
  
### <a name="data-autonomy"></a>Autonomía de datos  
Autonomía de datos implica la capacidad de una organización o grupo para administrar sus propios datos, incluidos decisiones administrativas sobre los datos y realizar cualquier tarea administrativa necesario sin necesidad de aprobación desde otra entidad.  
  
Autonomía de datos no impide que los administradores de servicio en el bosque de acceso a los datos. Por ejemplo, un grupo de investigación de una organización grande podría para poder administrar sus datos específicos del proyecto por sí mismos, pero no es necesario proteger los datos de otros administradores en el bosque.  
  
### <a name="service-isolation"></a>Aislamiento del servicio  
Aislar el servicio implica control exclusivo de la infraestructura de Active Directory. Los grupos que requieran aislar el servicio requieren que ningún administrador fuera del grupo puede interferir con el funcionamiento del servicio de directorio.  
  
Los requisitos legales u operativos normalmente crean una necesidad para aislar el servicio. Por ejemplo:  
  
-   Una empresa de fabricación tiene una aplicación crítica que controle el equipo en la planta de fábrica. No se puede permitir que las interrupciones en el servicio en otras partes de la red de la organización interferir con el funcionamiento de la planta de fábrica.  
  
-   Una empresa de hospedaje proporciona el servicio a varios clientes. Cada cliente necesita aislar el servicio para que cualquier interrupción del servicio que afecte a un cliente no afecta a los otros clientes.  
  
### <a name="service-autonomy"></a>Autonomía del servicio  
Autonomía implica la capacidad de administrar la infraestructura sin un requisito para control exclusivo; Por ejemplo, cuando un grupo quiere realizar cambios en la infraestructura (por ejemplo, agregar o quitar dominios, modificar el espacio de nombres del sistema de nombres de dominio (DNS) o la modificación del esquema) sin la autorización del propietario del bosque.  
  
Autonomía podría ser necesario dentro de una organización de un grupo que desea poder controlar el nivel de servicio de AD DS (agregar y quitar controladores de dominio, según sea necesario) o para un grupo en el que debe ser capaces de instalar aplicaciones habilitadas para directorio que requieren extensiones de esquema.  
  
## <a name="limited-connectivity"></a>Conectividad limitada  
Si un grupo de tu organización tiene redes que están separadas por los dispositivos que restringir o limitan la conectividad entre redes (por ejemplo, firewalls y dispositivos de traducción de direcciones de red (NAT)), esto puede afectar el diseño del bosque. Al identificar los requisitos de diseño de bosque, asegúrese de tener en cuenta las ubicaciones donde haya limitada conectividad de red. Esta información es necesaria para que pueda tomar decisiones en relación con el diseño de bosque.  
  


