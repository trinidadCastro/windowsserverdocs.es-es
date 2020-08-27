---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomía frente a aislamiento
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: eccbfc7767821a15d32d6aabef156861cb409f40
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941215"
---
# <a name="autonomy-vs-isolation"></a>Autonomía frente a aislamiento

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede diseñar la estructura lógica de Active Directory para lograr una de las siguientes acciones:

-   **Autonomía**. Implica un control independiente pero no exclusivo de un recurso. Al conseguir autonomía, los administradores tienen la autoridad necesaria para administrar los recursos de forma independiente. sin embargo, existen administradores con mayor autoridad que también tienen control sobre esos recursos y pueden tomar el control si es necesario. Puede diseñar la estructura lógica de Active Directory para lograr los siguientes tipos de autonomía:

    -   **Autonomía del servicio**. Este tipo de autonomía implica el control de toda o parte de la administración de servicios.

    -   **Autonomía**de los datos. Este tipo de autonomía implica el control sobre todos o parte de los datos almacenados en el directorio o en los equipos miembros Unidos al directorio.

-   **Aislamiento**. Implica el control independiente y exclusivo de un recurso. Al lograr el aislamiento, los administradores tienen la autoridad para administrar un recurso de forma independiente y ningún otro administrador puede quitar el control del recurso. Puede diseñar la estructura lógica de Active Directory para lograr los siguientes tipos de aislamiento:

    -   **Aislamiento del servicio**. Impide que los administradores (distintos de los administradores designados específicamente para controlar administración de servicios) controlen o interfieran con la administración de servicios.

    -   **Aislamiento de datos**. Impide que los administradores (que no sean los administradores designados específicamente para controlar o ver datos) controlen o vean un subconjunto de datos en el directorio o en los equipos miembro Unidos al directorio.

Los administradores que solo requieren autonomía aceptan que otros administradores que tienen una autoridad administrativa igual o superior tengan un mayor control sobre el servicio o la administración de datos. Los administradores que requieren aislamiento tienen control exclusivo sobre el servicio o la administración de datos. Crear un diseño para lograr autonomía es generalmente más económico que crear un diseño para lograr el aislamiento.

En Active Directory Domain Services (AD DS), los administradores pueden delegar la administración de servicios y la administración de datos para lograr autonomía o aislamiento entre organizaciones. La combinación de administración de servicios, administración de datos, autonomía y requisitos de aislamiento de una organización afecta a los contenedores de Active Directory que se usan para delegar la administración.

## <a name="isolation-and-autonomy-requirements"></a>Requisitos de aislamiento y autonomía
El número de bosques que debe implementar se basa en los requisitos de aislamiento y autonomía de cada grupo dentro de la organización. Para identificar los requisitos de diseño de bosque, debe identificar los requisitos de autonomía y aislamiento para todos los grupos de su organización. En concreto, debe identificar la necesidad de aislamiento de datos, autonomía de datos, aislamiento de servicio y autonomía de servicio. También debe identificar áreas de conectividad limitada en su organización.

### <a name="data-isolation"></a>Aislamiento de datos
El aislamiento de datos implica el control exclusivo sobre los datos por parte del grupo u organización que posee los datos. Es importante tener en cuenta que los administradores de servicios tienen la capacidad de tomar el control de un recurso fuera de los administradores de datos. Y los administradores de datos no tienen la capacidad de impedir que los administradores de servicios tengan acceso a los recursos que controlan. Por lo tanto, no puede lograr el aislamiento de datos cuando otro grupo dentro de la organización es responsable de la administración del servicio. Si un grupo requiere aislamiento de datos, ese grupo también debe asumir la responsabilidad de la administración del servicio.

Dado que los datos almacenados en AD DS y en equipos Unidos a AD DS no se pueden aislar de los administradores de servicios, la única manera de que un grupo dentro de una organización logre el aislamiento de datos completo es crear un bosque independiente para esos datos. Las organizaciones para las que las consecuencias de un ataque por parte de software malintencionado o por parte de un administrador de servicios convertido son importantes pueden optar por crear un bosque independiente para lograr el aislamiento de datos. Normalmente, los requisitos legales crean una necesidad de este tipo de aislamiento de datos. Por ejemplo:

-   La ley requiere una institución financiera para limitar el acceso a los datos que pertenecen a los clientes de una determinada jurisdicción a los usuarios, los equipos y los administradores que se encuentran en dicha jurisdicción. Aunque la institución confía en los administradores de servicios que trabajan fuera del área protegida, si se infringe la limitación de acceso, la institución ya no podrá hacer negocios en esa jurisdicción. Por lo tanto, la institución financiera debe aislar los datos de los administradores de servicios fuera de dicha jurisdicción. Tenga en cuenta que el cifrado no es siempre una alternativa a esta solución. Es posible que el cifrado no proteja los datos de los administradores de servicios.

-   La ley requiere un contratista de defensa para limitar el acceso a los datos del proyecto a un conjunto específico de usuarios. Aunque el contratista confía en los administradores de servicios que controlan los sistemas informáticos relacionados con otros proyectos, una infracción de esta limitación de acceso hará que el contratista pierda la empresa.

    > [!NOTE]
    > Si tiene un requisito de aislamiento de datos, debe decidir si necesita aislar los datos de los administradores de servicios o de los administradores de datos y de los usuarios normales. Si el requisito de aislamiento se basa en el aislamiento de los administradores de datos y los usuarios normales, puede utilizar las listas de control de acceso (ACL) para aislar los datos. Para los fines de este proceso de diseño, el aislamiento de los administradores de datos y los usuarios ordinarios no se considera un requisito de aislamiento de datos.

### <a name="data-autonomy"></a>Autonomía de los datos
La autonomía de los datos implica la capacidad de un grupo u organización de administrar sus propios datos, incluida la toma de decisiones administrativas sobre los datos y la realización de las tareas administrativas necesarias sin necesidad de aprobación de otra autoridad.

La autonomía de los datos no impide que los administradores de servicios del bosque tengan acceso a los datos. Por ejemplo, un grupo de investigación dentro de una organización grande podría querer administrar sus propios datos específicos del proyecto, pero no necesita proteger los datos de otros administradores en el bosque.

### <a name="service-isolation"></a>Aislamiento del servicio
El aislamiento de servicio implica el control exclusivo de la infraestructura de Active Directory. Los grupos que requieren el aislamiento del servicio requieren que ningún administrador fuera del grupo pueda interferir con el funcionamiento del servicio de directorio.

Los requisitos operativos o legales normalmente crean una necesidad de aislamiento de servicio. Por ejemplo:

-   Una empresa de fabricación tiene una aplicación crítica que controla los equipos de la fábrica. No se puede permitir que las interrupciones del servicio en otras partes de la red de la organización interfieran con el funcionamiento de la fábrica.

-   Una empresa de hospedaje proporciona servicio a varios clientes. Cada cliente requiere el aislamiento del servicio para que cualquier interrupción del servicio que afecte a un cliente no afecte a los demás clientes.

### <a name="service-autonomy"></a>Autonomía del servicio
La autonomía del servicio implica la capacidad de administrar la infraestructura sin necesidad de un control exclusivo; por ejemplo, cuando un grupo desea realizar cambios en la infraestructura (como agregar o quitar dominios, modificar el espacio de nombres del sistema de nombres de dominio (DNS) o modificar el esquema) sin la aprobación del propietario del bosque.

Es posible que sea necesaria la autonomía del servicio dentro de una organización para un grupo que desee poder controlar el nivel de servicio de AD DS (agregando y quitando controladores de dominio, según sea necesario) o para un grupo que necesite poder instalar aplicaciones habilitadas para el directorio que requieran extensiones de esquema.

## <a name="limited-connectivity"></a>Conectividad limitada
Si un grupo de la organización posee redes separadas por dispositivos que restringen o limitan la conectividad entre redes (como firewalls y dispositivos de traducción de direcciones de red (NAT)), esto puede afectar al diseño del bosque. Al identificar los requisitos de diseño de bosque, asegúrese de anotar las ubicaciones en las que tiene conectividad de red limitada. Esta información es necesaria para que pueda tomar decisiones sobre el diseño del bosque.



