---
title: Planear la migración siempre en VPN de acceso remoto
description: Migración de DirectAccess a VPN de Always On requiere un planeamiento adecuado determinar las fases de migración, lo que ayuda a identificar cualquier problema antes de que afecten a toda la organización.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 494dc7916b505991c22b07bec738c2300d660ec1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835676"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Planear la migración de DirectAccess a la VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

&#171;[ **Anterior:** Información general sobre el cliente de DirectAccess para la migración de VPN de Always On](da-always-on-migration-overview.md)<br>
&#187;[ **Siguiente:** Migrar a la VPN de Always On y retirada de DirectAccess](da-always-on-migration-deploy.md)


Migración de DirectAccess a VPN de Always On requiere un planeamiento adecuado determinar las fases de migración, lo que ayuda a identificar cualquier problema antes de que afecten a toda la organización. El objetivo principal de la migración es para que los usuarios mantener la conectividad remota a la oficina durante el proceso. Si realiza tareas fuera de servicio, puede producirse una condición de carrera, lo que deja a los usuarios remotos con ninguna manera de acceder a los recursos de empresa. Por lo tanto, Microsoft recomienda realizar una migración planeada, en paralelo de DirectAccess para VPN de Always On. Para obtener más información, consulte el [implementación de la migración de VPN de Always On](da-always-on-migration-deploy.md) sección.

La sección describe las ventajas de separar los usuarios para la migración, consideraciones sobre la configuración estándar y las mejoras de características de VPN de Always On. La fase de planeación de migración incluye:

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Cree anillos de migración
Anillos de migración se utilizan para dividir el esfuerzo de migración de cliente de VPN de Always On en varias fases. En el momento de que llegar a la última fase, el proceso debe ser bien probada y coherente.

Esta sección proporciona un enfoque para separar los usuarios en fases de migración y, a continuación, administrar esas fases. Independientemente del método de separación de fase de usuario que elija, mantener un único grupo de usuarios de VPN para facilitar la administración una vez completada la migración.

>[!NOTE] 
>La palabra _fase_ no está pensado para indicar que se trata de un proceso largo. Si se desplaza por cada fase en un par de días o un par de meses, Microsoft recomienda que aprovechar las ventajas de la migración en paralelo y usar un enfoque por fases.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Ventajas de dividir el esfuerzo de migración en varias fases

-   **Protección de interrupción masivo.** Al dividir una migración en fases, el número de personas que puede afectar un problema de migración generado es mucho menor.

-   **Mejora en el proceso o la comunicación de comentarios.** Idealmente, los usuarios ni siquiera advertirán que se ha producido la migración. Sin embargo, si su experiencia era menos óptima, comentarios de esos usos ofrece una oportunidad para mejorar el planeamiento y evitar problemas en el futuro.

### <a name="tips-for-building-your-migration-ring"></a>Sugerencias para compilar el anillo de migración

-   **Identifique los usuarios remotos.** Empiece separando los usuarios en dos depósitos: aquellos que con frecuencia entran en la oficina y aquellos que no lo hacen. El proceso de migración es el mismo para ambos grupos, pero es probable que tarden más tiempo para los clientes remotos recibir la actualización que aquellos que conectan con más frecuencia. Idealmente, cada fase de migración, debe incluir a los miembros de cada depósito.

-  **Dé prioridad a los usuarios.** Liderazgo y a otros usuarios de alto impacto son normalmente entre los usuarios de la última migrados. Al dar prioridad a los usuarios, sin embargo, tenga en cuenta su impacto en la productividad empresarial si la migración de sus equipos cliente que produjeron un error. Por ejemplo, si tuviera una calificación de 1 a 3, con 1 significa que el empleado no sería capaz de trabajo y 3, lo que significa que ninguna interrupción de trabajo inmediato, un analista de negocios con solo internas línea de negocio (LOB) las aplicaciones remotamente sería un 1, mientras que un vendedor con una nube  aplicación sería un 3.

-   **Migración de cada departamento o unidad de negocio en varias fases.** Microsoft recomienda encarecidamente que no se migra un departamento entero al mismo tiempo. Si debe producirse un problema, no desea que dificultan el trabajo remoto para todo el departamento. En su lugar, puede migrar cada departamento o unidad de negocio en al menos dos fases.

-   **Aumentar gradualmente los recuentos de usuarios.** Escenarios de migración más habituales comenzar con los miembros de la organización de TI y, a continuación, mover a los usuarios empresariales seguidos de liderazgo y a otros usuarios de alto impacto. Cada fase de migración implica normalmente progresivamente más personas. Por ejemplo, la primera fase puede incluir diez usuarios y el final del grupo puede incluir 5.000 usuarios. Para simplificar la implementación, crear un único grupo de seguridad de los usuarios de VPN y agregar usuarios a él como parte de su fase llega. De esta manera, terminará con un único grupo de usuarios de VPN a la que puede agregar a miembros en el futuro.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Paso siguiente

|Si desea...  |A continuación, vea...  |
|---------|---------|
|Iniciar la migración a la VPN de Always On     |[Migrar a la VPN de Always On y retirar DirectAccess](da-always-on-migration-deploy.md). Migración de DirectAccess a VPN de Always On, requiere un proceso específico para migrar a los clientes, lo que ayuda a minimiza las condiciones de carrera que surgen al realizar los pasos de migración fuera de servicio.         |
|Obtenga información acerca de las características de DirectAccess y VPN de Always On    |[Comparación de características de VPN o DirectAccess AlwaysOn](../vpn/vpn-map-da.md). En versiones anteriores de la arquitectura de VPN de Windows, limitaciones de la plataforma resultaba difícil proporcionar la funcionalidad crítica que es necesaria reemplazar DirectAccess (por ejemplo, las conexiones automáticas iniciadas antes de que los usuarios iniciar sesión). VPN de Always On, sin embargo, ha mitigado la mayoría de esas limitaciones o expandir la funcionalidad VPN más allá de las características de DirectAccess.         |



---