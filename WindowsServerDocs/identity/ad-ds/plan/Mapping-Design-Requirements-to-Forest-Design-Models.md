---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Requisitos de diseño de la asignación a los modelos de diseño de bosque
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 35d6322f053c7a02dc1df5430b28f771f57a1ad7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442570"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Requisitos de diseño de la asignación a los modelos de diseño de bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mayoría de los grupos de su organización puede compartir un único bosque de organización que administra un grupo de tecnología (TI) información único y que contiene las cuentas de usuario y recursos para todos los grupos que comparten el bosque. Este bosque compartido, denominado el bosque organizativo inicial, es la base del modelo de diseño de bosque para la organización.  

Dado que el bosque organizativo inicial puede alojar varios grupos de la organización, el propietario del bosque debe establecer los acuerdos de nivel de servicio con cada grupo para que todas las partes comprender lo que se espera de ellos. Esto protege a los grupos individuales y el propietario del bosque mediante el establecimiento de las expectativas de servicio acordado.  

Si no todos los grupos de su organización pueden compartir un único bosque de la organización, debe expandir el diseño de bosque para satisfacer las necesidades de los diferentes grupos. Esto implica la identificación de los requisitos de diseño que se aplican a los grupos según sus necesidades de autonomía y aislamiento y si tienen una conectividad limitada de red y, a continuación, que identifica el modelo de bosque que puede usar para dar cabida a las requisitos. En la tabla siguiente se enumera los escenarios del modelo de diseño de bosque según la autonomía, aislamiento y factores de conectividad. Después de identificar el escenario de diseño de bosque que mejor coincida con sus requisitos, determine si necesita decisiones adicionales para cumplir con las especificaciones de diseño.  

> [!NOTE]  
> Si aparece un factor como N/D, no es una consideración porque otros requisitos también dar cabida a ese factor.  

|Escenario|Conectividad limitada|Aislamiento de los datos|Autonomía de datos|Aislamiento del servicio|Autonomía del servicio|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Escenario 1: Unirse a un bosque existente para la autonomía de datos](#BKMK_1)|No|No|Sí|No|No|  
|[Escenario 2: Usar un dominio o bosque organizativo de la autonomía del servicio](#BKMK_2)|No|No|N/D|No|Sí|  
|[Escenario 3: Usar un bosque organizativa o el bosque de recursos para el aislamiento del servicio](#BKMK_3)|No|No|N/D|Sí|N/D|  
|[Escenario 4: Utilice un bosque de la organización o bosque de acceso restringido para el aislamiento de datos](#BKMK_4)|N/D|Sí|N/D|N/D|N/D|  
|[Escenario 5: Usar un bosque organizativo o volver a configurar el firewall para la conectividad limitada](#BKMK_5)|Sí|No|N/D|No|No|  
|[Escenario 6: Usar un dominio o bosque organizativo y volver a configurar el firewall para la autonomía del servicio con conectividad limitada](#BKMK_6)|Sí|No|N/D|No|Sí|  
|[Escenario 7: Usar un bosque de recursos y volver a configurar el firewall para el aislamiento del servicio con conectividad limitada](#BKMK_7)|Sí|No|N/D|Sí|N/D|  

## <a name="BKMK_1"></a>Escenario 1: Unirse a un bosque existente para la autonomía de datos  

Simplemente puede satisfacer un requisito para la autonomía de datos hospeda el grupo de unidades organizativas (OU) en un bosque existente de la organización. Delegar el control sobre las unidades organizativas a los administradores de datos de ese grupo para lograr la autonomía de datos. Para obtener más información acerca de cómo delegar el control mediante el uso de unidades organizativas, consulte [creación de un diseño de unidad organizativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Escenario 2: Usar un dominio o bosque organizativo de la autonomía del servicio  

Si un grupo de la organización identifica la autonomía del servicio como un requisito, se recomienda que primero reconsiderar este requisito. Para lograr la autonomía del servicio crea más sobrecarga de administración y los costes adicionales para la organización. Asegúrese de que el requisito de la autonomía del servicio no es simplemente por comodidad y que pueden justificar los costos implicados en este requisito se cumple.  
  
Puede cumplir un requisito para la autonomía del servicio mediante una de las siguientes acciones:  

- Creación de un bosque organizativo. Coloque los usuarios, grupos y equipos para el grupo que requiere la autonomía del servicio en un bosque independiente de la organización. Asignar a un usuario individual de ese grupo para que sea el propietario del bosque. Si el grupo necesita acceso o recurso compartido de recursos con otros bosques de la organización, puede establecer una relación de confianza entre el bosque de la organización y los otros bosques.  

- Uso de dominios de la organización. Coloque los usuarios, grupos y equipos en un dominio independiente en un bosque existente de la organización. Este modelo proporciona sólo la autonomía del servicio de nivel de dominio y no para la autonomía del servicio completo, servicio de aislamiento, o el aislamiento de datos.  

Para obtener más información sobre el uso de dominios de la organización, consulte [mediante el modelo de bosque de dominio organizativo](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Escenario 3: Usar un bosque organizativa o el bosque de recursos para el aislamiento del servicio  

Puede cumplir un requisito de aislamiento del servicio mediante una de las siguientes acciones:  

- Uso de un bosque de la organización. Coloque los usuarios, grupos y equipos para el grupo que requiere el aislamiento del servicio en un bosque independiente de la organización. Asignar a un usuario individual de ese grupo para que sea el propietario del bosque. Si el grupo necesita acceso o recurso compartido de recursos con otros bosques de la organización, puede establecer una relación de confianza entre el bosque de la organización y los otros bosques. Sin embargo, no se recomienda este enfoque porque el acceso a los recursos a través de los grupos universales está muy restringido en escenarios de confianza de bosque.  

- Uso de un bosque de recursos. Coloque los recursos y cuentas de servicio en un bosque de recursos independiente, mantener las cuentas de usuario en un bosque existente de la organización. Si es necesario, las cuentas alternativas pueden crearse en el bosque de recursos para acceder a recursos en el bosque de recursos si el bosque organizativo deja de estar disponible. Las cuentas alternativas deben tener la autoridad necesaria para iniciar sesión en el bosque de recursos y mantener el control de los recursos hasta que se vuelve a conectar el bosque organizativo.  

   Establecer una relación de confianza entre el recurso y bosques de la organización, para que los usuarios puedan acceder a los recursos en el bosque al usar sus cuentas de usuario normales. Esta configuración permite una administración centralizada de cuentas de usuario al permitir a los usuarios revertir a una cuenta alternativa en el bosque de recursos si el bosque organizativo deja de estar disponible.  

Consideraciones para el aislamiento del servicio incluyen lo siguiente:

- Bosques creados para el aislamiento del servicio pueden confiar en dominios de otros bosques, pero no puede contener usuarios de otros bosques en los grupos de administradores de servicio. Si los usuarios de otros bosques se incluyen en los grupos administrativos en el bosque aislado, la seguridad del bosque aislado puede verse afectada porque los administradores de servicios en el bosque no tiene control exclusivo.  

- Siempre y cuando los controladores de dominio están accesibles en una red, están sujetos a ataques (por ejemplo, ataques de denegación de servicio) del software malintencionado en esa red. Puede hacer lo siguiente para protegerse contra la posibilidad de un ataque:  

   - Controladores de dominio de host solo en redes que se consideran seguros.  

   - Limitar el acceso a la red o redes que aloja los controladores de dominio.  

- Aislamiento del servicio requiere la creación de un bosque adicional. Evalúe si el costo de mantenimiento de la infraestructura para admitir el bosque adicional supera los costos asociados con la pérdida de acceso a los recursos debido a un bosque de organización que no está disponible.  

## <a name="BKMK_4"></a>Escenario 4: Utilice un bosque de la organización o bosque de acceso restringido para el aislamiento de datos  

Puede conseguir el aislamiento de datos realizando una de las siguientes acciones:  

- Uso de un bosque de la organización. Coloque los usuarios, grupos y equipos para el grupo que requiere el aislamiento de los datos en un bosque independiente de la organización. Asignar a un usuario individual de ese grupo para que sea el propietario del bosque. Si el grupo necesita acceso o recurso compartido de recursos con otros bosques de la organización, establecer una relación de confianza entre el bosque de la organización y los otros bosques. Solo los usuarios que requieren acceso a la información clasificada existen en el nuevo bosque de la organización. Los usuarios tienen una cuenta que pueden usar para acceder a ambas clasifica los datos en su propio bosque y los datos sin clasificar en otros bosques por medio de relaciones de confianza.  

- Uso de un bosque de acceso restringido. Se trata de un bosque independiente que contiene los datos restringidos y las cuentas de usuario que se usan para acceder a los datos. Cuentas de usuario independientes se mantienen en los bosques existentes de organización que se usan para tener acceso a los recursos de la red sin restricciones. No hay confianzas se crean entre el bosque de acceso restringido y de otros bosques de la empresa. Puede restringir aún más el bosque mediante la implementación del bosque en una red física independiente, por lo que no puede conectarse a otros bosques. Si implementa el bosque en una red distinta, los usuarios deben tener dos estaciones de trabajo: uno para tener acceso a los bosques restringido y uno para tener acceso a las áreas de la red nonrestricted.  

Consideraciones para la creación de bosques para el aislamiento de datos incluyen lo siguiente:  

- Bosques organizativas creados para el aislamiento de datos pueden confiar en dominios de otros bosques, pero los usuarios de otros bosques no deben incluirse en cualquiera de las siguientes acciones:  

  - Grupos responsables de la administración de servicios o grupos que pueden administrar la pertenencia a grupos del Administrador de servicio  

  - Grupos que tienen control administrativo sobre los equipos que almacenan los datos protegidos  

  - Los grupos que tienen acceso a datos protegidos o grupos que son responsables de la administración de objetos de usuario o grupo que tienen acceso a datos protegidos  

    Si los usuarios de otro bosque se incluyen en ninguno de estos grupos, podría provocar un riesgo en el otro bosque a un riesgo en el bosque aislado y la divulgación de datos protegidos.  

- Otros bosques pueden configurarse para que confíe en el bosque organizativo creado para el aislamiento de datos para que los usuarios del bosque aislado pueden tener acceso a recursos en otros bosques. Sin embargo, los usuarios del bosque aislado interactivamente nunca deben iniciar sesión en estaciones de trabajo en el bosque que confía. El equipo en el bosque que confía podrían estar en peligro el software malintencionado y puede usarse para capturar las credenciales de inicio de sesión del usuario.  

   > [!NOTE]
   > Para evitar que los servidores en un bosque que confía suplantando a los usuarios del bosque aislado y, a continuación, obtener acceso a recursos en el bosque aislado, el propietario del bosque puede deshabilitar la autenticación delegada o usar la característica de delegación restringida. Para obtener más información acerca de la autenticación delegada y la delegación restringida, consulte [Delegar autenticación](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Es posible que deba establecer un firewall entre el bosque de la organización y los otros bosques de la organización para limitar el acceso de usuario a la información fuera de su bosque.  

- Aunque la creación de un bosque independiente permite el aislamiento de los datos, siempre y cuando los controladores de dominio en el bosque aislado y equipos de esa información de host protegido son accesibles en una red, están sujetos a ataques iniciados desde equipos en esa red. Las organizaciones que decida que el riesgo de ataque es demasiado alto o que la consecuencia de una infracción de seguridad o ataque es demasiado grande que se necesitan limitar el acceso a la red o redes que hospedan los controladores de dominio y los equipos que hospedan los datos protegidos . Limitar el acceso puede realizarse mediante el uso de tecnologías como firewalls y seguridad de protocolo Internet (IPsec). En casos extremos, las organizaciones pueden elegir mantener los datos protegidos en una red independiente que no se tiene ninguna conexión física a ninguna otra red de la organización.  

   > [!NOTE]  
   > Si existe alguna conectividad de red entre un bosque de acceso restringido y otra red, existe la posibilidad de datos en el área que se transmite a la otra red restringido.  

## <a name="BKMK_5"></a>Escenario 5: Usar un bosque organizativo o volver a configurar el firewall para la conectividad limitada  

Para satisfacer un requisito de conectividad limitada, puede realizar una de las siguientes acciones:  

- Coloque los usuarios en un bosque existente de la organización y, a continuación, abra el firewall lo suficientemente para permitir el tráfico de Active Directory pasar a través.  

- Utilice un bosque organizativo. Coloque los usuarios, grupos y equipos para el grupo para el que se limita la conectividad en un bosque independiente de la organización. Asignar a un usuario individual de ese grupo para que sea el propietario del bosque. El bosque de la organización proporciona un entorno independiente en el otro lado del servidor de seguridad. El bosque incluye las cuentas de usuario y los recursos que se administran en el bosque, por lo que los usuarios no es necesario ir a través del firewall para realizar sus tareas diarias. Determinados usuarios o aplicaciones podrían tener necesidades especiales que requieren la capacidad para pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente, abra las interfaces adecuadas en el servidor de seguridad, incluidos aquellos necesarios para las relaciones de confianza funcionar.  

Para obtener más información acerca de cómo configurar los firewalls para su uso con los servicios de dominio de Active Directory (AD DS), consulte [Active Directory en redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Escenario 6: Usar un dominio o bosque organizativo y volver a configurar el firewall para la autonomía del servicio con conectividad limitada  

Si un grupo de la organización identifica la autonomía del servicio como un requisito, se recomienda que primero reconsiderar este requisito. Para lograr la autonomía del servicio crea más sobrecarga de administración y los costes adicionales para la organización. Asegúrese de que el requisito de la autonomía del servicio no es simplemente por comodidad y que pueden justificar los costos implicados en este requisito se cumple.  

Si la conectividad limitada es un problema y tiene un requisito para la autonomía del servicio, puede realizar uno de los siguientes:  

- Utilice un bosque organizativo. Coloque los usuarios, grupos y equipos para el grupo que requiere la autonomía del servicio en un bosque independiente de la organización. Asignar a un usuario individual de ese grupo para que sea el propietario del bosque. El bosque de la organización proporciona un entorno independiente en el otro lado del servidor de seguridad. El bosque incluye las cuentas de usuario y los recursos que se administran en el bosque, por lo que los usuarios no es necesario ir a través del firewall para realizar sus tareas diarias. Determinados usuarios o aplicaciones podrían tener necesidades especiales que requieren la capacidad para pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente, abra las interfaces adecuadas en el servidor de seguridad, incluidos aquellos necesarios para las relaciones de confianza funcionar.  

- Coloque los usuarios, grupos y equipos en un dominio independiente en un bosque existente de la organización. Este modelo proporciona sólo la autonomía del servicio de nivel de dominio y no para la autonomía del servicio completo, servicio de aislamiento, o el aislamiento de datos. Otros grupos del bosque deben confiar en los administradores de servicios del nuevo dominio en la misma medida que confían en el propietario del bosque. Por este motivo, no se recomienda este enfoque. Para obtener más información sobre el uso de dominios de la organización, consulte [mediante el modelo de bosque de dominio organizativo](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

También debe abrir el firewall lo suficientemente para permitir el tráfico pase a través de Active Directory. Para obtener más información acerca de cómo configurar los firewalls para su uso con AD DS, consulte [Active Directory en redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Escenario 7: Usar un bosque de recursos y volver a configurar el firewall para el aislamiento del servicio con conectividad limitada  

Si la conectividad limitada es un problema, y tiene un requisito de aislamiento del servicio, puede realizar uno de los siguientes:  

- Utilice un bosque organizativo. Coloque los usuarios, grupos y equipos para el grupo que requiere el aislamiento del servicio en un bosque independiente de la organización. Asignar a un usuario individual de ese grupo para que sea el propietario del bosque. El bosque de la organización proporciona un entorno independiente en el otro lado del servidor de seguridad. El bosque incluye las cuentas de usuario y los recursos que se administran en el bosque, por lo que los usuarios no es necesario ir a través del firewall para realizar sus tareas diarias. Determinados usuarios o aplicaciones podrían tener necesidades especiales que requieren la capacidad para pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente, abra las interfaces adecuadas en el servidor de seguridad, incluidos aquellos necesarios para las relaciones de confianza funcionar.  

- Utilice un bosque de recursos. Coloque los recursos y cuentas de servicio en un bosque de recursos independiente, mantener las cuentas de usuario en un bosque existente de la organización. Podría ser necesario crear algunas cuentas de usuario alternativo en el bosque de recursos para mantener el acceso para el bosque de recursos si el bosque organizativo deja de estar disponible. Las cuentas alternativas deben tener la autoridad necesaria para iniciar sesión en el bosque de recursos y mantener el control de los recursos hasta que se vuelve a conectar el bosque organizativo.  

   Establecer una relación de confianza entre el recurso y bosques de la organización, para que los usuarios puedan acceder a los recursos en el bosque al usar sus cuentas de usuario normales. Esta configuración permite una administración centralizada de cuentas de usuario al permitir a los usuarios revertir a una cuenta alternativa en el bosque de recursos si el bosque organizativo deja de estar disponible.  

Consideraciones para el aislamiento del servicio incluyen lo siguiente:  

- Bosques creados para el aislamiento del servicio pueden confiar en dominios de otros bosques, pero no puede contener usuarios de otros bosques en los grupos de administradores de servicio. Si los usuarios de otros bosques se incluyen en los grupos administrativos en el bosque aislado, la seguridad del bosque aislado puede verse afectada porque los administradores de servicios en el bosque no tiene control exclusivo.  

- Siempre y cuando los controladores de dominio están accesibles en una red, están sujetos a ataques (por ejemplo, ataques de denegación de servicio) de los equipos en esa red. Puede hacer lo siguiente para protegerse contra la posibilidad de un ataque:  

   - Controladores de dominio de host solo en redes que se consideran seguros.  

   - Limitar el acceso a la red o redes que aloja los controladores de dominio.  

- Aislamiento del servicio requiere la creación de un bosque adicional. Evalúe si el costo de mantenimiento de la infraestructura para admitir el bosque adicional supera los costos asociados con la pérdida de acceso a los recursos debido a un bosque de organización que no está disponible.  

   Determinados usuarios o aplicaciones podrían tener necesidades especiales que requieren la capacidad para pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente, abra las interfaces adecuadas en el servidor de seguridad, incluidos aquellos necesarios para las relaciones de confianza funcionar.  

Para obtener más información acerca de cómo configurar los firewalls para su uso con AD DS, consulte [Active Directory en redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  
