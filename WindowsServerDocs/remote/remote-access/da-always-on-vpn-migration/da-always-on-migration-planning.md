---
title: Planeación de la migración de VPN Always On de acceso remoto
description: La migración de DirectAccess a Always On VPN requiere un planeamiento adecuado para determinar las fases de migración, lo que ayuda a identificar los problemas antes de que afecten a toda la organización.
manager: dougkim
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/29/2018
ms.openlocfilehash: 57b44b90213ca756666b83aa934bc6fd42fa609b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951430"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Planeación de la migración de DirectAccess a Always On VPN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

&#171; [ **anterior:** Introducción a la migración de DIRECTACCESS a Always on VPN](da-always-on-migration-overview.md)<br>
&#187; [ **siguiente:** migrar a Always on VPN y retirar DirectAccess](da-always-on-migration-deploy.md)


La migración de DirectAccess a Always On VPN requiere un planeamiento adecuado para determinar las fases de migración, lo que ayuda a identificar los problemas antes de que afecten a toda la organización. El objetivo principal de la migración es que los usuarios mantengan la conectividad remota a la oficina a lo largo del proceso. Si realiza tareas desordenadas, puede producirse una condición de carrera, de modo que los usuarios remotos no podrán acceder a los recursos de la empresa. Por lo tanto, Microsoft recomienda realizar una migración en paralelo planeada de DirectAccess a Always On VPN. Para obtener más información, consulte la sección implementación de la [migración de VPN Always on](da-always-on-migration-deploy.md) .

En esta sección se describen las ventajas de separar usuarios para la migración, las consideraciones de configuración estándar y Always On mejoras de las características de VPN. La fase de planeación de la migración incluye:

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)]

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)]

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Anillos de migración de compilación
Los anillos de migración se utilizan para dividir el Always On el trabajo de migración de clientes VPN en varias fases. En el momento en que llegue a la última fase, el proceso debe estar bien probado y coherente.

En esta sección se proporciona un enfoque para separar usuarios en fases de migración y, a continuación, administrar esas fases. Independientemente del método de separación de fase de usuario que elija, mantenga un solo grupo de usuarios de VPN para facilitar la administración cuando la migración haya finalizado.

>[!NOTE]
>La palabra _fase_ no está pensada para indicar que se trata de un proceso largo. Tanto si se desplaza por una fase en un par de días como durante un par de meses, Microsoft recomienda aprovechar las ventajas de la migración en paralelo y usar un enfoque por fases.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Ventajas de dividir el trabajo de migración en varias fases

-   **Protección contra interrupción masiva.** Al dividir una migración en fases, el número de personas a las que un problema generado por la migración puede afectar es mucho menor.

-   **Mejora en el proceso o la comunicación de los comentarios.** Idealmente, los usuarios no observaban siquiera que se produjo la migración. Sin embargo, si su experiencia fuera inferior a la óptima, los comentarios de estos usos le ofrecen la oportunidad de mejorar el planeamiento y evitar problemas en el futuro.

### <a name="tips-for-building-your-migration-ring"></a>Sugerencias para crear el anillo de migración

-   **Identifique usuarios remotos.** Empiece separando los usuarios en dos cubos: los que suelen entrar en la oficina y los que no lo hacen. El proceso de migración es el mismo para ambos grupos, pero es probable que tarde más tiempo para que los clientes remotos reciban la actualización que para los que se conectan con mayor frecuencia. Cada fase de migración, idealmente, debe incluir miembros de cada depósito.

-  **Priorizar a los usuarios.** El liderazgo y otros usuarios de gran impacto suelen estar entre los últimos usuarios migrados. Sin embargo, al priorizar a los usuarios, considere su impacto en la productividad empresarial si se produjera un error en la migración de su equipo cliente. Por ejemplo, si tuviera una clasificación de 1 a 3, con 1 significa que el empleado no podía trabajar y 3, lo que significa que no había ninguna interrupción inmediata del trabajo, un analista de negocios que solo use aplicaciones de línea de negocio (LOB) internas de forma remota sería un 1, mientras que un vendedor que usa una aplicación en la nube sería un 3.

-   **Migre cada departamento o unidad de negocio en varias fases.** Microsoft recomienda encarecidamente no migrar todo un departamento al mismo tiempo. Si se produce un problema, no querrá que se dificulte el trabajo remoto para todo el Departamento. En su lugar, migre cada departamento o unidad de negocio en al menos dos fases.

-   **Aumentar gradualmente los recuentos de usuarios.** Los escenarios de migración más habituales comienzan con los miembros de la organización de ti y, a continuación, se mueven a usuarios empresariales seguidos de liderazgo y otros usuarios de gran impacto. Cada fase de migración implica normalmente más personas progresivamente. Por ejemplo, la primera fase puede incluir diez usuarios y el grupo final puede incluir 5.000 usuarios. Para simplificar la implementación, cree un grupo de seguridad de usuarios de VPN único y agréguele usuarios a medida que llegue su fase. De esta manera, terminará con un único grupo de usuarios de VPN al que puede Agregar miembros en el futuro.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Paso siguiente

|Si desea...  |A continuación, vea...  |
|---------|---------|
|Inicio de la migración a Always On VPN     |[Migre a Always on VPN y retire DirectAccess](da-always-on-migration-deploy.md). La migración de DirectAccess a Always On VPN requiere un proceso específico para migrar los clientes, lo que ayuda a minimizar las condiciones de carrera que surgen al seguir los pasos de migración sin orden.         |
|Obtener información acerca de las características de Always On VPN y DirectAccess    |[Comparación de características de Always on VPN y DirectAccess](../vpn/vpn-map-da.md). En versiones anteriores de la arquitectura de VPN de Windows, las limitaciones de la plataforma dificultaban proporcionar la funcionalidad crítica necesaria para reemplazar DirectAccess (como las conexiones automáticas iniciadas antes de que los usuarios iniciaran sesión). Sin embargo, Always On VPN ha mitigado la mayoría de esas limitaciones o ha ampliado la funcionalidad de VPN más allá de las capacidades de DirectAccess.         |



---