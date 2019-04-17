---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: "Requisitos de diseño de la asignación a los modelos de diseño del bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 48a2b03c6e29afcca565e861383a831050ef4233
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Requisitos de diseño de la asignación a los modelos de diseño del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mayoría de los grupos en la organización puede compartir un único bosque organizativo que administra un grupo de tecnología de información única y que contiene las cuentas de usuario y los recursos para todos los grupos que comparten el bosque. Este bosque compartido, llamada el bosque organizativo inicial, es el fundamento del modelo de diseño de bosque de la organización.  
  
Dado que el bosque organizativo inicial puede hospedar varios grupos de la organización, el propietario del bosque debe establecer acuerdos de nivel de servicio con cada grupo para que todas las partes comprender lo que se espera de ellos. Protege los grupos individuales y el propietario del bosque con el establecimiento de las expectativas del acuerdo de servicio.  
  
Si no todos los grupos en la organización pueden compartir un único bosque organizativo, debe expandir el diseño de bosque para satisfacer las necesidades de los distintos grupos. Esto implica que identifica los requisitos de diseño que se aplican a los grupos en función de sus necesidades de autonomía y aislamiento y tenga o no tienen una red de conectividad limitada y, a continuación, identifica el modelo de bosque que puedes usar para dar cabida a estos requisitos. La siguiente tabla enumera los escenarios de modelo de diseño de bosque en función de los factores de conectividad, autonomía y aislamiento. Después de identificar el escenario de diseño de bosque que mejor se adapte a tus requisitos, determinar si necesitas decisiones adicionales para satisfacer las especificaciones de diseño.  
  
> [!NOTE]  
> Si aparece un factor como n /, no es una consideración porque otros requisitos dar cabida también a ese factor.  
  
|Escenario|Conectividad limitada|Aislamiento de datos|Autonomía de datos|Aislamiento del servicio|Autonomía del servicio|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Escenario 1: Unirse a un bosque existente para autonomía de datos](#BKMK_1)|No|No|Sí|No|No|  
|[Escenario 2: Usar un dominio o bosque organizativa para autonomía de servicio](#BKMK_2)|No|No|N/D|No|Sí|  
|[Escenario 3: Usar un bosque organizativa o el bosque de recursos para el aislamiento de servicio](#BKMK_3)|No|No|N/D|Sí|N/D|  
|[Escenario 4: Usar un bosque organizativa o el bosque de acceso restringido para el aislamiento de datos](#BKMK_4)|N/D|Sí|N/D|N/D|N/D|  
|[Escenario 5: Usar un bosque de organización, o volver a configurar el firewall para la conectividad limitada](#BKMK_5)|Sí|No|N/D|No|No|  
|[El escenario 6: Usar un dominio o bosque organizativa y volver a configurar el firewall de autonomía de servicio con conectividad limitada](#BKMK_6)|Sí|No|N/D|No|Sí|  
|[Escenario de 7: Usar un bosque de recursos y volver a configurar el firewall para aislar el servicio con conectividad limitada](#BKMK_7)|Sí|No|N/D|Sí|N/D|  
  
## <a name="BKMK_1"></a>Escenario 1: Unirse a un bosque existente para autonomía de datos  
Basta con que aloja el grupo en unidades organizativas (OU en un bosque organizativo existente) puede cumplir un requisito para autonomía de datos. Delegar control sobre las unidades organizativas a los administradores de datos de ese grupo para lograr autonomía de datos. Para obtener más información acerca de la delegación de control mediante el uso de unidades organizativas, consulta [crear un diseño de la unidad organizativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Escenario 2: Usar un dominio o bosque organizativa para autonomía de servicio  
Si un grupo en la organización identifica autonomía como requisito, te recomendamos que primero reconsiderar este requisito. Conseguir autonomía crea más sobrecarga de administración y los costes adicionales para la organización. Asegúrate de que el requisito de autonomía no es simplemente para mayor comodidad y que puede justifican el costo que implica cumplir este requisito.  
  
Puede cumplir un requisito para autonomía del servicio mediante una de las siguientes acciones:  
  
-   Creación de un bosque de la organización. Coloca los usuarios, grupos y equipos del grupo que requiere autonomía del servicio en un bosque organizativo independiente. Asignar a una persona del grupo sea el propietario del bosque. Si se necesita el grupo a los recursos de acceder o compartir con otros bosques de la organización, puede establecer una relación de confianza entre su organización bosque y los demás bosques.  
  
-   Uso de dominios de organización. Coloca los usuarios, grupos y equipos en un dominio distinto en un bosque organizativo existente. Este modelo proporciona solo autonomía de servicio de nivel de dominio y no para autonomía de servicio completo, el mantenimiento aislamiento o aislamiento de datos.  
  
Para obtener más información sobre el uso de dominios de organización, consulta [con el modelo de bosque de dominio organizativa](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
## <a name="BKMK_3"></a>Escenario 3: Usar un bosque organizativa o el bosque de recursos para el aislamiento de servicio  
Puede cumplir un requisito para aislar el servicio mediante una de las siguientes acciones:  
  
-   Uso de un bosque de la organización. Coloca los usuarios, grupos y equipos del grupo que requiere aislar el servicio en un bosque organizativo independiente. Asignar a una persona del grupo sea el propietario del bosque. Si se necesita el grupo a los recursos de acceder o compartir con otros bosques de la organización, puede establecer una relación de confianza entre su organización bosque y los demás bosques. Sin embargo, no recomendamos este enfoque porque el acceso a los recursos mediante grupos universales está muy restringido en escenarios de confianza de bosque.  
  
-   Uso de un bosque de recursos. Coloca los recursos y las cuentas de servicio en un bosque de recursos independiente, mantener las cuentas de usuario en un bosque organizativo existente. Si es necesario, cuentas alternativas pueden crearse en el bosque de recursos para acceder a recursos en el bosque de recursos si el bosque de la organización no está disponible. Las cuentas alternativas deben tener la autoridad necesaria para iniciar sesión en el bosque de recursos y mantener el control de los recursos hasta que se vuelva a conectar el bosque de la organización.  
  
    Establecer una relación de confianza entre el recurso y bosques organizativas, para que los usuarios pueden acceder a los recursos en el bosque mientras con sus cuentas de usuario normal. Esta configuración permite una administración centralizada de cuentas de usuario, permitiendo que los usuarios puedan volver a alternativas cuentas en el bosque de recursos si el bosque de la organización no está disponible.  
  
Para el aislamiento de servicio de las siguientes consideraciones:  
  
-   Bosques creados para aislar el servicio pueden confiar en dominios de otros bosques, pero no deben incluir los usuarios de otros bosques de los grupos de administradores de servicio. Si los usuarios de otros bosques se incluyen en los grupos administrativos en el bosque aislado, la seguridad del bosque aislado puede verse afectada porque los administradores de servicio en el bosque no tienen control exclusivo.  
  
-   Como los controladores de dominio son accesibles en una red, están sujetos a ataques (por ejemplo, ataques de denegación de servicio) de software malintencionado en esa red. Puedes hacer lo siguiente para evitar la posibilidad de un ataque:  
  
    -   Controladores de dominio del host solo en las redes que se consideran seguros.  
  
    -   Limitar el acceso a la red o las redes que hospeda los controladores de dominio.  
  
-   Aislar el servicio requiere la creación de un bosque adicional. Evaluar si el costo de mantenimiento de la infraestructura para admitir el bosque adicional supera los costos asociados con la pérdida de acceso a recursos debido a un bosque de organización no está disponible.  
  
## <a name="BKMK_4"></a>Escenario 4: Usar un bosque organizativa o el bosque de acceso restringido para el aislamiento de datos  
Puedes lograr el aislamiento de datos mediante una de las siguientes acciones:  
  
-   Uso de un bosque de la organización. Coloca los usuarios, grupos y equipos del grupo que requiere el aislamiento de datos en un bosque organizativo independiente. Asignar a una persona del grupo sea el propietario del bosque. Si se necesita el grupo a los recursos de acceder o compartir con otros bosques de la organización, establecer una relación de confianza entre el bosque organizativo y los demás bosques. Solo los usuarios que necesitan acceder a la información clasificada existan en el nuevo bosque organizativo. Los usuarios tienen una cuenta que usan los datos de acceso clasificado en su propio bosque y datos sin clasificar de otros bosques por medio de las relaciones de confianza.  
  
-   Uso de un bosque de acceso restringido. Se trata de un bosque independiente que contiene los datos restringidos y las cuentas de usuario que se usan para acceder a esos datos. Cuentas de usuario independientes se mantienen en los bosques organizativos existentes que se usan para acceder a los recursos de la red sin restricciones. No se crean confianzas entre el bosque de acceso restringido y demás bosques de la empresa. Puede restringir aún más el bosque mediante la implementación de bosque en una red física independiente, para que no puede conectarse a otros bosques. Si implementas el bosque en una red separada, los usuarios deben tener dos estaciones de trabajo: uno para tener acceso a del bosque restringido y otro para acceder a las áreas nonrestricted de la red.  
  
Para crear bosques para el aislamiento de datos de las siguientes consideraciones:  
  
-   Bosques organizativas creados para el aislamiento de datos pueden confiar en dominios de otros bosques, pero los usuarios de otros bosques no deben incluirse en cualquiera de las siguientes acciones:  
  
    -   Grupos responsables de administración de servicios o grupos que pueden administrar la pertenencia a grupos de administrador de servicio  
  
    -   Los grupos que tienen control administrativo sobre los equipos que almacenan datos protegidos  
  
    -   Los grupos que tienen acceso a los datos protegidos o grupos que son responsables de la administración de objetos de usuario o grupo que tienen acceso a los datos protegidos.  
  
    Si los usuarios de otro bosque se incluyen en cualquiera de estos grupos, podría provocar un compromiso de otro bosque en peligro el bosque aislado y divulgación de datos protegidos.  
  
-   Otros bosques pueden configurarse para que confíe en el bosque de organización creado para el aislamiento de datos para que los usuarios del bosque aislado pueden acceder a recursos en otros bosques. Sin embargo, los usuarios del bosque aislado interactivamente nunca deben iniciar sesión en estaciones de trabajo en el bosque de confianza. El equipo en el bosque de confianza potencialmente puede estar en peligro el software malintencionado y puede usarse para capturar las credenciales de inicio de sesión del usuario.  
  
    > [!NOTE]  
    > Para evitar que los servidores de un bosque de confianza suplantan la identidad de los usuarios del bosque aislado y, a continuación, tener acceso a recursos en el bosque aislado, el propietario del bosque puede deshabilitar la autenticación delegada o utilizar la característica de delegación restringida. Para obtener más información sobre la autenticación delegada y delegación restringida, consulta la delegación de autenticación ([https://go.microsoft.com/fwlink/?LinkId=106614](https://go.microsoft.com/fwlink/?LinkId=106614)).  
  
-   Es posible que debas establecer un firewall entre el bosque organizativo y los demás bosques en la organización para limitar el acceso de usuario a información fuera de su bosque.  
  
-   Aunque la creación de un bosque independiente permite el aislamiento de datos, siempre que los controladores de dominio en el bosque aislado y equipos de esa información de host protegido sean accesibles en una red, están sujetos a ataques contra los equipos de esa red. Las organizaciones que decidir que el riesgo de ataque es demasiado alto o que es demasiado grande como consecuencia de una infracción de seguridad o de ataque se necesitan limitar el acceso a las redes que hospedan los controladores de dominio y los equipos que hospedan los datos protegidos. Limitar el acceso puede realizarse mediante el uso de tecnologías como firewalls y seguridad de protocolo de Internet (IPsec). En casos extremos, las organizaciones pueden optar por mantener los datos protegidos en una red independiente que no tiene ninguna conexión física a cualquier otra red en la organización.  
  
    > [!NOTE]  
    > Si no existe conectividad de red entre un bosque de acceso restringido y otra red, existe la posibilidad de datos en el área que se transmitirán a la otra red restringido.  
  
## <a name="BKMK_5"></a>Escenario 5: Usar un bosque de organización, o volver a configurar el firewall para la conectividad limitada  
Para cumplir un requisito de conectividad limitada, puedes realizar una de las siguientes acciones:  
  
-   Colocar los usuarios en un bosque organizativo existente y abre el firewall suficiente para permitir el tráfico de Active Directory pasar.  
  
-   Usa un bosque de la organización. Colocar los usuarios, grupos y equipos del grupo para el que está limitada conectividad en un bosque organizativo independiente. Asignar a una persona del grupo sea el propietario del bosque. El bosque organizativo proporciona un entorno independiente en el otro lado del servidor de seguridad. El bosque incluye cuentas de usuario y los recursos que se administran en el bosque, para que los usuarios no es necesario ir a través del firewall para realizar sus tareas diarias. Determinados usuarios o aplicaciones pueden tener necesidades especiales que requieran la capacidad para pasar a través del firewall ponerse en contacto con otros bosques. Puedes dar respuesta estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, los necesarios para que las relaciones de confianza funcionar incluidos.  
  
Para obtener más información sobre la configuración de Firewall para su uso con los servicios de dominio de Active Directory (AD DS), vea en redes segmentados los servidores de seguridad de Active Directory ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_6"></a>El escenario 6: Usar un dominio o bosque organizativa y volver a configurar el firewall de autonomía de servicio con conectividad limitada  
Si un grupo en la organización identifica autonomía como requisito, te recomendamos que primero reconsiderar este requisito. Conseguir autonomía crea más sobrecarga de administración y los costes adicionales para la organización. Asegúrate de que el requisito de autonomía no es simplemente para mayor comodidad y que puede justifican el costo que implica cumplir este requisito.  
  
Si la conectividad limitada es un problema y tiene un requisito para autonomía del servicio, puedes realizar una de las siguientes acciones:  
  
-   Usa un bosque de la organización. Coloca los usuarios, grupos y equipos del grupo que requiere autonomía del servicio en un bosque organizativo independiente. Asignar a una persona del grupo sea el propietario del bosque. El bosque organizativo proporciona un entorno independiente en el otro lado del servidor de seguridad. El bosque incluye cuentas de usuario y los recursos que se administran en el bosque, para que los usuarios no es necesario ir a través del firewall para realizar sus tareas diarias. Determinados usuarios o aplicaciones pueden tener necesidades especiales que requieran la capacidad para pasar a través del firewall ponerse en contacto con otros bosques. Puedes dar respuesta estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, los necesarios para que las relaciones de confianza funcionar incluidos.  
  
-   Coloca los usuarios, grupos y equipos en un dominio distinto en un bosque organizativo existente. Este modelo proporciona solo autonomía de servicio de nivel de dominio y no para autonomía de servicio completo, el mantenimiento aislamiento o aislamiento de datos. Otros grupos en el bosque deben confiar en los administradores de servicios de dominio de nuevo para el mismo grado que el propietario de bosque de confianza. Por este motivo, no recomendamos este enfoque. Para obtener más información sobre el uso de dominios de organización, consulta [con el modelo de bosque de dominio organizativa](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
También debes abrir el firewall suficiente para permitir el tráfico de Active Directory pasar. Para obtener más información sobre la configuración de Firewall para su uso con AD DS, vea en redes segmentados los servidores de seguridad de Active Directory ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_7"></a>Escenario de 7: Usar un bosque de recursos y volver a configurar el firewall para aislar el servicio con conectividad limitada  
Si la conectividad limitada es un problema y tienes un requisito para aislar el servicio, puedes realizar una de las siguientes acciones:  
  
-   Usa un bosque de la organización. Coloca los usuarios, grupos y equipos del grupo que requiere aislar el servicio en un bosque organizativo independiente. Asignar a una persona del grupo sea el propietario del bosque. El bosque organizativo proporciona un entorno independiente en el otro lado del servidor de seguridad. El bosque incluye cuentas de usuario y los recursos que se administran en el bosque, para que los usuarios no es necesario ir a través del firewall para realizar sus tareas diarias. Determinados usuarios o aplicaciones pueden tener necesidades especiales que requieran la capacidad para pasar a través del firewall ponerse en contacto con otros bosques. Puedes dar respuesta estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, los necesarios para que las relaciones de confianza funcionar incluidos.  
  
-   Usa un bosque de recursos. Coloca los recursos y las cuentas de servicio en un bosque de recursos independiente, mantener las cuentas de usuario en un bosque organizativo existente. Podrían ser necesario para crear algunas cuentas de usuario alternativa en el bosque de recursos para mantener el acceso al bosque de recursos si el bosque de la organización no está disponible. Las cuentas alternativas deben tener la autoridad necesaria para iniciar sesión en el bosque de recursos y mantener el control de los recursos hasta que se vuelva a conectar el bosque de la organización.  
  
    Establecer una relación de confianza entre el recurso y bosques organizativas, para que los usuarios pueden acceder a los recursos en el bosque mientras con sus cuentas de usuario normal. Esta configuración permite una administración centralizada de cuentas de usuario, permitiendo que los usuarios puedan volver a alternativas cuentas en el bosque de recursos si el bosque de la organización no está disponible.  
  
Para el aislamiento de servicio de las siguientes consideraciones:  
  
-   Bosques creados para aislar el servicio pueden confiar en dominios de otros bosques, pero no deben incluir los usuarios de otros bosques de los grupos de administradores de servicio. Si los usuarios de otros bosques se incluyen en los grupos administrativos en el bosque aislado, la seguridad del bosque aislado puede verse afectada porque los administradores de servicio en el bosque no tienen control exclusivo.  
  
-   Como los controladores de dominio son accesibles en una red, están sujetos a ataques (por ejemplo, ataques de denegación de servicio) desde equipos en esa red. Puedes hacer lo siguiente para evitar la posibilidad de un ataque:  
  
    -   Controladores de dominio del host solo en las redes que se consideran seguros.  
  
    -   Limitar el acceso a la red o las redes que hospeda los controladores de dominio.  
  
-   Aislar el servicio requiere la creación de un bosque adicional. Evaluar si el costo de mantenimiento de la infraestructura para admitir el bosque adicional supera los costos asociados con la pérdida de acceso a recursos debido a un bosque de organización no está disponible.  
  
    Determinados usuarios o aplicaciones pueden tener necesidades especiales que requieran la capacidad para pasar a través del firewall ponerse en contacto con otros bosques. Puedes dar respuesta estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, los necesarios para que las relaciones de confianza funcionar incluidos.  
  
Para obtener más información sobre la configuración de Firewall para su uso con AD DS, vea en redes segmentados los servidores de seguridad de Active Directory ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  


