---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Asignar los requisitos de diseño a los modelos de diseño de bosque
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d65b03dc255de5523c48c2bb9359530b8e7c3167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408766"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Asignar los requisitos de diseño a los modelos de diseño de bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La mayoría de los grupos de su organización pueden compartir un solo bosque de la organización administrado por un único grupo de tecnologías de la información (TI) y que contiene las cuentas de usuario y los recursos de todos los grupos que comparten el bosque. Este bosque compartido, denominado el bosque de la organización inicial, es la base del modelo de diseño de bosque de la organización.  

Dado que el bosque de la organización inicial puede hospedar varios grupos en la organización, el propietario del bosque debe establecer acuerdos de nivel de servicio con cada grupo para que todas las partes sepan lo que se espera. Esto protege tanto los grupos individuales como el propietario del bosque mediante el establecimiento de las expectativas de servicio acordadas.  

Si no todos los grupos de su organización pueden compartir un solo bosque de la organización, debe expandir el diseño del bosque para adaptarse a las necesidades de los diferentes grupos. Esto implica la identificación de los requisitos de diseño que se aplican a los grupos en función de sus necesidades de autonomía y aislamiento, y si tienen una red de conectividad limitada y, a continuación, la identificación del modelo de bosque que se puede usar para acomodarlos. satisfacer. En la tabla siguiente se enumeran los escenarios del modelo de diseño de bosque basados en los factores de autonomía, aislamiento y conectividad. Después de identificar el escenario de diseño de bosque que mejor se adapte a sus requisitos, determine si necesita tomar decisiones adicionales para cumplir sus especificaciones de diseño.  

> [!NOTE]  
> Si un factor aparece como N/A, no es una consideración porque otros requisitos también se ajustan a ese factor.  

|Escenario|Conectividad limitada|Aislamiento de datos|Autonomía de los datos|Aislamiento del servicio|Autonomía del servicio|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Escenario 1: unir un bosque existente para la autonomía de los datos](#BKMK_1)|Sin|Sin|Sí|Sin|Sin|  
|[Escenario 2: usar un bosque o dominio de la organización para la autonomía del servicio](#BKMK_2)|Sin|Sin|N/D|Sin|Sí|  
|[Escenario 3: usar un bosque de la organización o un bosque de recursos para el aislamiento del servicio](#BKMK_3)|Sin|Sin|N/D|Sí|N/D|  
|[Escenario 4: usar un bosque de la organización o un bosque de acceso restringido para el aislamiento de datos](#BKMK_4)|N/D|Sí|N/D|N/D|N/D|  
|[Escenario 5: usar un bosque de la organización o volver a configurar el firewall para una conectividad limitada](#BKMK_5)|Sí|Sin|N/D|Sin|Sin|  
|[Escenario 6: usar un bosque o dominio de la organización y volver a configurar el firewall para la autonomía del servicio con conectividad limitada](#BKMK_6)|Sí|Sin|N/D|Sin|Sí|  
|[Escenario 7: usar un bosque de recursos y volver a configurar el firewall para el aislamiento de servicio con conectividad limitada](#BKMK_7)|Sí|Sin|N/D|Sí|N/D|  

## <a name="BKMK_1"></a>Escenario 1: unir un bosque existente para la autonomía de los datos  

Puede satisfacer un requisito de autonomía de datos con solo hospedar el grupo en unidades organizativas (OU) en un bosque de la organización existente. Delegue el control sobre las unidades organizativas en administradores de datos de ese grupo para lograr la autonomía de los datos. Para obtener más información sobre cómo delegar el control mediante el uso de unidades organizativas, consulte [crear un diseño de unidad organizativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Escenario 2: usar un bosque o dominio de la organización para la autonomía del servicio  

Si un grupo de su organización identifica la autonomía del servicio como requisito, se recomienda que primero reconsidere este requisito. Lograr la autonomía del servicio crea más sobrecarga de administración y costos adicionales para la organización. Asegúrese de que el requisito de autonomía de servicio no sea simplemente por comodidad y de que pueda justificar los costos implicados en el cumplimiento de este requisito.  
  
Puede cumplir un requisito para la autonomía del servicio mediante una de las siguientes acciones:  

- Crear un bosque de la organización. Coloque los usuarios, los grupos y los equipos del grupo que requiera la autonomía del servicio en un bosque de la organización independiente. Asigne un individuo de ese grupo para que sea el propietario del bosque. Si el grupo necesita tener acceso a recursos o compartirlos con otros bosques de la organización, pueden establecer una relación de confianza entre el bosque de la organización y los otros bosques.  

- Usar dominios de la organización. Coloque los usuarios, grupos y equipos en un dominio independiente en un bosque de la organización existente. Este modelo proporciona únicamente autonomía de servicio en el nivel de dominio y no para la autonomía total del servicio, el aislamiento del servicio o el aislamiento de datos.  

Para obtener más información sobre el uso de dominios de la organización, vea [usar el modelo de bosque de dominio de la organización](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Escenario 3: usar un bosque de la organización o un bosque de recursos para el aislamiento del servicio  

Puede cumplir un requisito para el aislamiento del servicio mediante una de las siguientes acciones:  

- Usar un bosque de la organización. Coloque los usuarios, los grupos y los equipos del grupo que requiera el aislamiento del servicio en un bosque de la organización independiente. Asigne un individuo de ese grupo para que sea el propietario del bosque. Si el grupo necesita tener acceso a recursos o compartirlos con otros bosques de la organización, pueden establecer una relación de confianza entre el bosque de la organización y los otros bosques. Sin embargo, no se recomienda este enfoque porque el acceso a los recursos a través de grupos universales está muy restringido en escenarios de confianza de bosque.  

- Usar un bosque de recursos. Coloque los recursos y las cuentas de servicio en un bosque de recursos independiente, manteniendo las cuentas de usuario en un bosque de la organización existente. Si es necesario, se pueden crear cuentas alternativas en el bosque de recursos para tener acceso a los recursos del bosque de recursos si el bosque de la organización deja de estar disponible. Las cuentas alternativas deben tener la autoridad necesaria para iniciar sesión en el bosque de recursos y mantener el control de los recursos hasta que el bosque de la organización vuelva a estar en línea.  

   Establezca una confianza entre el recurso y los bosques de la organización, de modo que los usuarios puedan tener acceso a los recursos del bosque mientras usan sus cuentas de usuario normales. Esta configuración permite la administración centralizada de las cuentas de usuario, a la vez que permite a los usuarios revertir a cuentas alternativas en el bosque de recursos si el bosque de la organización deja de estar disponible.  

Entre las consideraciones sobre el aislamiento de servicio se incluyen las siguientes:

- Los bosques creados para el aislamiento de servicio pueden confiar en dominios de otros bosques, pero no deben incluir usuarios de otros bosques en sus grupos de administradores de servicios. Si los usuarios de otros bosques están incluidos en los grupos administrativos del bosque aislado, es posible que la seguridad del bosque aislado se vea comprometida porque los administradores de servicios del bosque no tienen control exclusivo.  

- Siempre que se pueda tener acceso a los controladores de dominio en una red, están sujetos a ataques (por ejemplo, ataques por denegación de servicio) del software malintencionado de esa red. Puede hacer lo siguiente para protegerse frente a la posibilidad de un ataque:  

   - Hospede controladores de dominio solo en redes que se consideren seguras.  

   - Limite el acceso a la red o redes que hospedan los controladores de dominio.  

- El aislamiento del servicio requiere la creación de un bosque adicional. Evalúe si el costo del mantenimiento de la infraestructura para admitir el bosque adicional supera los costos asociados con la pérdida de acceso a los recursos debido a que un bosque de la organización no está disponible.  

## <a name="BKMK_4"></a>Escenario 4: usar un bosque de la organización o un bosque de acceso restringido para el aislamiento de datos  

Puede lograr el aislamiento de datos realizando una de las siguientes acciones:  

- Usar un bosque de la organización. Coloque los usuarios, los grupos y los equipos del grupo que requiera el aislamiento de datos en un bosque de la organización independiente. Asigne un individuo de ese grupo para que sea el propietario del bosque. Si el grupo necesita tener acceso a recursos y compartirlos con otros bosques de la organización, establezca una relación de confianza entre el bosque de la organización y los demás bosques. Solo los usuarios que necesitan acceso a la información clasificada existen en el nuevo bosque de la organización. Los usuarios tienen una cuenta que usan para tener acceso a los datos clasificados en su propio bosque y datos sin clasificar en otros bosques mediante relaciones de confianza.  

- Usar un bosque de acceso restringido. Se trata de un bosque independiente que contiene los datos restringidos y las cuentas de usuario que se usan para obtener acceso a los datos. Las cuentas de usuario independientes se mantienen en los bosques de la organización existentes que se usan para tener acceso a los recursos sin restricciones de la red. No se crea ninguna confianza entre el bosque de acceso restringido y otros bosques de la empresa. Puede restringir aún más el bosque implementando el bosque en una red física independiente para que no pueda conectarse a otros bosques. Si implementa el bosque en una red independiente, los usuarios deben tener dos estaciones de trabajo: una para tener acceso al bosque restringido y otra para tener acceso a las áreas no restringidas de la red.  

Entre las consideraciones para crear bosques para el aislamiento de datos se incluyen las siguientes:  

- Los bosques de la organización creados para el aislamiento de datos pueden confiar en dominios de otros bosques, pero los usuarios de otros bosques no deben incluirse en ninguno de los siguientes elementos:  

  - Grupos responsables de la administración de servicios o los grupos que pueden administrar la pertenencia de los grupos de administradores de servicios  

  - Grupos que tienen control administrativo sobre los equipos que almacenan datos protegidos  

  - Grupos que tienen acceso a los datos o grupos protegidos que son responsables de la administración de objetos de usuario o objetos de grupo que tienen acceso a datos protegidos  

    Si los usuarios de otro bosque están incluidos en cualquiera de estos grupos, un riesgo del otro bosque podría poner en peligro el bosque aislado y la revelación de datos protegidos.  

- Otros bosques se pueden configurar para confiar en el bosque de la organización creado para el aislamiento de datos, de modo que los usuarios del bosque aislado puedan tener acceso a los recursos de otros bosques. Sin embargo, los usuarios del bosque aislado nunca deben iniciar sesión de forma interactiva en las estaciones de trabajo del bosque que confía. El equipo del bosque que confía podría verse comprometido por software malintencionado y se puede usar para capturar las credenciales de inicio de sesión del usuario.  

   > [!NOTE]
   > Para evitar que los servidores de un bosque que confía en suplantar a los usuarios del bosque aislado y, a continuación, tener acceso a los recursos del bosque aislado, el propietario del bosque puede deshabilitar la autenticación delegada o usar la característica de delegación restringida. Para obtener más información sobre la autenticación delegada y la delegación restringida, vea [delegar la autenticación](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Es posible que tenga que establecer un firewall entre el bosque de la organización y los otros bosques de la organización para limitar el acceso de los usuarios a información fuera de su bosque.  

- Aunque la creación de un bosque independiente habilita el aislamiento de datos, siempre que los controladores de dominio del bosque aislado y los equipos que hospedan información protegida sean accesibles en una red, están sujetos a ataques iniciados desde equipos de esa red. Las organizaciones que deciden que el riesgo de ataque es demasiado alto o que la consecuencia de un ataque o una infracción de seguridad es demasiado grande para limitar el acceso a la red o redes que hospedan los controladores de dominio y los equipos que hospedan los datos protegidos. . La limitación del acceso puede realizarse mediante tecnologías como firewalls y el protocolo de seguridad de Internet (IPsec). En casos extremos, las organizaciones pueden optar por mantener los datos protegidos en una red independiente que no tiene ninguna conexión física con ninguna otra red de la organización.  

   > [!NOTE]  
   > Si existe conectividad de red entre un bosque de acceso restringido y otra red, existe la posibilidad de que los datos del área restringida se transmitan a la otra red.  

## <a name="BKMK_5"></a>Escenario 5: usar un bosque de la organización o volver a configurar el firewall para una conectividad limitada  

Para cumplir un requisito de conectividad limitado, puede realizar una de las siguientes acciones:  

- Coloque los usuarios en un bosque de la organización existente y, a continuación, abra el Firewall suficiente para permitir el paso del tráfico Active Directory.  

- Usar un bosque de la organización. Coloque los usuarios, grupos y equipos del grupo para los que la conectividad está limitada en un bosque de la organización independiente. Asigne un individuo de ese grupo para que sea el propietario del bosque. El bosque de la organización proporciona un entorno independiente en el otro lado del firewall. El bosque incluye las cuentas de usuario y los recursos que se administran en el bosque, de modo que los usuarios no tienen que pasar por el firewall para realizar sus tareas diarias. Es posible que determinados usuarios o aplicaciones tengan necesidades especiales que requieran la capacidad de pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, incluidas las necesarias para que funcionen las confianzas.  

Para obtener más información acerca de cómo configurar firewalls para su uso con Active Directory Domain Services (AD DS), consulte [Active Directory en redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Escenario 6: usar un bosque o dominio de la organización y volver a configurar el firewall para la autonomía del servicio con conectividad limitada  

Si un grupo de su organización identifica la autonomía del servicio como requisito, se recomienda que primero reconsidere este requisito. Lograr la autonomía del servicio crea más sobrecarga de administración y costos adicionales para la organización. Asegúrese de que el requisito de autonomía de servicio no sea simplemente por comodidad y de que pueda justificar los costos implicados en el cumplimiento de este requisito.  

Si la conectividad limitada es un problema y tiene un requisito para la autonomía del servicio, puede realizar una de las siguientes acciones:  

- Usar un bosque de la organización. Coloque los usuarios, los grupos y los equipos del grupo que requiera la autonomía del servicio en un bosque de la organización independiente. Asigne un individuo de ese grupo para que sea el propietario del bosque. El bosque de la organización proporciona un entorno independiente en el otro lado del firewall. El bosque incluye las cuentas de usuario y los recursos que se administran en el bosque, de modo que los usuarios no tienen que pasar por el firewall para realizar sus tareas diarias. Es posible que determinados usuarios o aplicaciones tengan necesidades especiales que requieran la capacidad de pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, incluidas las necesarias para que funcionen las confianzas.  

- Coloque los usuarios, grupos y equipos en un dominio independiente en un bosque de la organización existente. Este modelo proporciona únicamente autonomía de servicio en el nivel de dominio y no para la autonomía total del servicio, el aislamiento del servicio o el aislamiento de datos. Otros grupos del bosque deben confiar en los administradores de servicios del nuevo dominio hasta el mismo grado en que confían en el propietario del bosque. Por esta razón, no se recomienda este enfoque. Para obtener más información sobre el uso de dominios de la organización, vea [usar el modelo de bosque de dominio de la organización](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

También debe abrir el Firewall suficiente para permitir el paso del tráfico Active Directory. Para obtener más información sobre la configuración de firewalls para su uso con AD DS, consulte [Active Directory en redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Escenario 7: usar un bosque de recursos y volver a configurar el firewall para el aislamiento de servicio con conectividad limitada  

Si la conectividad limitada es un problema y tiene un requisito para el aislamiento del servicio, puede realizar una de las siguientes acciones:  

- Usar un bosque de la organización. Coloque los usuarios, los grupos y los equipos del grupo que requiera el aislamiento del servicio en un bosque de la organización independiente. Asigne un individuo de ese grupo para que sea el propietario del bosque. El bosque de la organización proporciona un entorno independiente en el otro lado del firewall. El bosque incluye las cuentas de usuario y los recursos que se administran en el bosque, de modo que los usuarios no tienen que pasar por el firewall para realizar sus tareas diarias. Es posible que determinados usuarios o aplicaciones tengan necesidades especiales que requieran la capacidad de pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, incluidas las necesarias para que funcionen las confianzas.  

- Use un bosque de recursos. Coloque los recursos y las cuentas de servicio en un bosque de recursos independiente, manteniendo las cuentas de usuario en un bosque de la organización existente. Es posible que sea necesario crear algunas cuentas de usuario alternativas en el bosque de recursos para mantener el acceso al bosque de recursos si el bosque de la organización deja de estar disponible. Las cuentas alternativas deben tener la autoridad necesaria para iniciar sesión en el bosque de recursos y mantener el control de los recursos hasta que el bosque de la organización vuelva a estar en línea.  

   Establezca una confianza entre el recurso y los bosques de la organización, de modo que los usuarios puedan tener acceso a los recursos del bosque mientras usan sus cuentas de usuario normales. Esta configuración permite la administración centralizada de las cuentas de usuario, a la vez que permite a los usuarios revertir a cuentas alternativas en el bosque de recursos si el bosque de la organización deja de estar disponible.  

Entre las consideraciones sobre el aislamiento de servicio se incluyen las siguientes:  

- Los bosques creados para el aislamiento de servicio pueden confiar en dominios de otros bosques, pero no deben incluir usuarios de otros bosques en sus grupos de administradores de servicios. Si los usuarios de otros bosques están incluidos en los grupos administrativos del bosque aislado, es posible que la seguridad del bosque aislado se vea comprometida porque los administradores de servicios del bosque no tienen control exclusivo.  

- Siempre que se pueda tener acceso a los controladores de dominio en una red, están sujetos a ataques (como ataques por denegación de servicio) de los equipos de la red. Puede hacer lo siguiente para protegerse frente a la posibilidad de un ataque:  

   - Hospede controladores de dominio solo en redes que se consideren seguras.  

   - Limite el acceso a la red o redes que hospedan los controladores de dominio.  

- El aislamiento del servicio requiere la creación de un bosque adicional. Evalúe si el costo del mantenimiento de la infraestructura para admitir el bosque adicional supera los costos asociados con la pérdida de acceso a los recursos debido a que un bosque de la organización no está disponible.  

   Es posible que determinados usuarios o aplicaciones tengan necesidades especiales que requieran la capacidad de pasar a través del firewall para ponerse en contacto con otros bosques. Puede abordar estas necesidades individualmente abriendo las interfaces adecuadas en el firewall, incluidas las necesarias para que funcionen las confianzas.  

Para obtener más información sobre la configuración de firewalls para su uso con AD DS, consulte [Active Directory en redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  
