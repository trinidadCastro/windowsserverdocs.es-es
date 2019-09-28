---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planeación de compromiso
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ee1416a00fc0d347b7e05cb12c83f3d3532d693f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360141"
---
# <a name="planning-for-compromise"></a>Planeación de compromiso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

@no__t 0Law: Nadie cree que se produzca nada malo, hasta que lo hace. * - [10 leyes inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Los planes de recuperación ante desastres de muchas organizaciones se centran en la recuperación de desastres o errores regionales que provocan la pérdida de servicios informáticos. Sin embargo, cuando se trabaja con clientes comprometidos, a menudo encontramos que la recuperación de un riesgo intencionado está ausente en sus planes de recuperación ante desastres. Esto es especialmente cierto cuando el riesgo es un robo de propiedad intelectual o destrucción intencional que aprovecha los límites lógicos (por ejemplo, la destrucción de todos los dominios de Active Directory o todos los servidores) en lugar de los límites físicos (por ejemplo, destrucción de un centro de recursos). Aunque una organización puede tener planes de respuesta a incidentes que definan las actividades iniciales que deben llevarse a cabo cuando se detecta un riesgo, estos planes omiten a menudo los pasos para recuperarse de un riesgo que afecte a toda la infraestructura de computación.  
  
Dado que Active Directory proporciona capacidades enriquecidas de administración de identidades y acceso para usuarios, servidores, estaciones de trabajo y aplicaciones, son destinadas invariablemente a los atacantes. Si un atacante obtiene acceso con privilegios elevados a un dominio de Active Directory o a un controlador de dominio, se puede aprovechar el acceso para obtener acceso, controlar o incluso destruir todo el bosque de Active Directory.  
  
En este documento se han analizado algunos de los ataques más comunes contra Windows y Active Directory y contramedidas que puede implementar para reducir la superficie expuesta a ataques, pero la única forma de garantizar la recuperación en caso de que se produzca un riesgo completo de Active Directory es preparado para poner en peligro antes de que se produzca. Esta sección se centra en los detalles de implementación técnicos menos importantes que en las secciones anteriores de este documento y más en recomendaciones de alto nivel que puede usar para crear un enfoque integral y completo para proteger y administrar el nivel crítico de su organización. activos empresariales y de ti.  
  
Tanto si su infraestructura nunca ha sufrido un ataque, ha superado los intentos de infracciones o ha succumbeddo a ataques y se ha puesto en peligro por completo, debe planear la realidad inevitable de que volverá a atacar y de nuevo. No es posible evitar ataques, pero es posible que sea posible evitar infracciones significativas o un riesgo mayorista. Cada organización debe evaluar detenidamente los programas de administración de riesgos existentes y realizar los ajustes necesarios para ayudar a reducir el nivel global de vulnerabilidades al realizar inversiones equilibradas en prevención, detección, contención y recuperación.  
  
Para crear defensas eficaces y, al mismo tiempo, proporcionar servicios a los usuarios y empresas que dependen de la infraestructura y las aplicaciones, es posible que tenga que considerar nuevas formas de evitar, detectar y contener riesgos en su entorno y, a continuación, recuperarse de el riesgo. Es posible que los enfoques y las recomendaciones de este documento no le ayuden a reparar una instalación en peligro Active Directory, pero puede ayudarle a proteger su siguiente.  
  
Las recomendaciones para la recuperación de un bosque de Active Directory se presentan en [Windows Server 2012: Planeación de Active Directory la recuperación del bosque @ no__t-0. Es posible que pueda evitar que el nuevo entorno se ponga en peligro por completo, pero incluso si no lo hace, tendrá herramientas para recuperar y volver a obtener el control de su entorno.  
  
## <a name="rethinking-the-approach"></a>Replanteamiento del enfoque  
*Law número ocho: La dificultad de defender una red es directamente proporcional a su complejidad.* - [10 leyes inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Por lo general, es aceptable que, si un atacante obtiene acceso de sistema, administrador, raíz o equivalente a un equipo, independientemente del sistema operativo, ese equipo ya no se considera de confianza, independientemente de cuántos esfuerzos se realicen para "limpiar" el integrado. Active Directory no es diferente. Si un atacante ha obtenido acceso con privilegios a un controlador de dominio o a una cuenta con privilegios elevados en Active Directory, a menos que tenga un registro de todas las modificaciones que realiza el atacante o una copia de seguridad en buen estado, nunca puede restaurar el directorio a un Estado de confianza.  
  
Cuando un atacante pone en peligro un servidor miembro o una estación de trabajo, el equipo deja de ser de confianza, pero los servidores y las estaciones de trabajo que no están en peligro pueden seguir siendo de confianza. Poner en peligro un equipo no implica que todos los equipos estén en peligro.  
  
Sin embargo, en un dominio de Active Directory, todos los controladores de dominio hospedan réplicas de la misma base de datos AD DS. Si un solo controlador de dominio se ve comprometido y un atacante modifica el AD DS base de datos, esas modificaciones se replican en todos los demás controladores de dominio del dominio y, en función de la partición en la que se realizan las modificaciones, el bosque. Incluso si vuelve a instalar todos los controladores de dominio del bosque, simplemente está reinstalando los hosts en los que reside la base de datos de AD DS. Las modificaciones malintencionadas en Active Directory se replicarán en los controladores de dominio recién instalados con la misma facilidad con la que se replicarán en los controladores de dominio que se han ejecutado durante años.  
  
En la evaluación de entornos en peligro, normalmente encontramos que lo que se consideró la primera brecha "evento" se desencadenó después de semanas, meses o incluso años después de que los atacantes habían puesto en peligro inicialmente el entorno. Normalmente, los atacantes obtuvieron las credenciales para las cuentas con privilegios elevados antes de que se detectara una infracción, y aprovechan esas cuentas para poner en peligro el directorio, los controladores de dominio, los servidores miembro, las estaciones de trabajo e incluso las que no son de Windows. sistemas.  
  
Estos hallazgos son coherentes con varios hallazgos en el informe de investigación de infracciones de datos 2012 de Verizon, que indica lo siguiente:  
  
-   98 por ciento de las infracciones de datos que corvienen de agentes externos  
  
-   el 85 por ciento de las infracciones de datos tardó semanas o más en detectarse  
  
-   un tercero detectó el 92 por ciento de los incidentes y  
  
-   el 97 por ciento de las infracciones se evitan a pesar de los controles simples o intermedios.  
  
Un riesgo para el grado descrito anteriormente es realmente irreparable y el Consejo estándar para "aplanar y recompilar" cada sistema en riesgo no es factible ni siquiera posible si Active Directory se ha puesto en peligro o se ha destruido. Incluso si se restaura a un estado bueno conocido, no se eliminan los errores que permiten que el entorno se ponga en peligro en primer lugar.  
  
Aunque debe defender todas las facetas de la infraestructura, un atacante solo necesita encontrar suficientes defectos en las defensas para llegar a su objetivo deseado. Si su entorno es relativamente sencillo y puro, y históricamente bien administrado, implementar las recomendaciones proporcionadas anteriormente en este documento puede ser una propuesta sencilla.  
  
Sin embargo, hemos descubierto que el entorno más antiguo, más grande y más complejo, lo más probable es que las recomendaciones de este documento sean inviables o incluso imposibles de implementar. Es mucho más difícil proteger una infraestructura después de que se inicie de nuevo y construir un entorno que sea resistente a los ataques y los riesgos. Pero, como se indicó anteriormente, no es una pequeña tarea para recompilar todo un bosque de Active Directory. Por estas razones, se recomienda un enfoque más orientado a la protección de los bosques Active Directory.  
  
En lugar de centrarse en y tratar de corregir todas las cosas que están "rotas", considere un enfoque en el que prioriza en función de lo que sea más importante para su negocio y en su infraestructura. En lugar de intentar corregir un entorno rellenado con sistemas y aplicaciones mal configurados, considere la posibilidad de crear un nuevo entorno seguro y pequeño en el que pueda trasladar de forma segura los usuarios, sistemas e información más importantes para su comerciales.  
  
En esta sección, se describe un enfoque por el que puede crear un bosque AD DS puro que actúe como "bote de vida" o "celda segura" para su infraestructura de negocio principal. Un bosque puro es simplemente un bosque de Active Directory recién instalado que normalmente está limitado por el tamaño y el ámbito, y que se basa en el uso de sistemas operativos actuales, aplicaciones y con los principios descritos en [reducción del ataque de Active Directory. Superficie](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Mediante la implementación de las opciones de configuración recomendadas en un bosque recién creado, puede crear una instalación de AD DS que se cree desde cero con las prácticas y configuraciones seguras, y puede reducir los desafíos que acompañan a los sistemas heredados de soporte técnico. programas. Aunque las instrucciones detalladas para el diseño y la implementación de una instalación de AD DS original están fuera del ámbito de este documento, debe seguir algunos principios y directrices generales para crear una "celda segura" en la que puede hospedar sus más críticas. circula. Estas instrucciones son las siguientes:  
  
1.  Identifique los principios para separar y proteger los recursos críticos.  
  
2.  Defina un plan de migración limitado y basado en riesgos.  
  
3.  Aproveche las migraciones "no migratorias" cuando sea necesario.  
  
4.  Implemente la "destrucción creativa".  
  
5.  Aísle las aplicaciones y los sistemas heredados.  
  
6.  Simplifique la seguridad para los usuarios finales.  
  
### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificación de los principios para separar y proteger los recursos críticos  

Las características del entorno original que cree para hospedar los recursos críticos pueden variar considerablemente. Por ejemplo, puede optar por crear un bosque original en el que migrar solo los usuarios VIP y los datos confidenciales a los que solo pueden acceder los usuarios. Puede crear un bosque puro en el que migrar no solo usuarios VIP, sino que implementa como un bosque administrativo, implementando los principios descritos en [reducir la superficie expuesta a ataques de Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) para crear cuentas administrativas seguras. y hosts que se pueden usar para administrar los bosques heredados desde el bosque original. Podría implementar un bosque "creado específicamente" que hospede cuentas de VIP, cuentas con privilegios y sistemas que requieran una seguridad adicional, como servidores que ejecuten Active Directory servicios de Certificate Server (AD CS) con el único objetivo de segregarlas de menos seguras. ellos. Por último, puede implementar un bosque original que se convierte en la ubicación de facto para todos los nuevos usuarios, sistemas, aplicaciones y datos, lo que le permite retirar finalmente su bosque heredado a través de desgaste.  
  
Independientemente de si el bosque original contiene una serie de usuarios y sistemas, o si es la base de una migración más agresiva, debe seguir estos principios en su planificación:  
  
1.  Supongamos que los bosques heredados se han puesto en peligro.  
  
2.  No configure un entorno puro para confiar en un bosque heredado, aunque puede configurar un entorno heredado para que confíe en un bosque original.  
  
3.  No migre cuentas de usuario o grupos de un bosque heredado a un entorno puro si existe la posibilidad de que las pertenencias a grupos de las cuentas, el historial de SID u otros atributos se hayan modificado de forma malintencionada. En su lugar, utilice enfoques "no migratorias" para rellenar un bosque original. (Los enfoques no migratorias se describen más adelante en esta sección).  
  
4.  No migre equipos de bosques heredados a bosques puros. Implemente servidores recién instalados en el bosque original, instale aplicaciones en los servidores recién instalados y migre los datos de la aplicación a los sistemas recién instalados. En el caso de los servidores de archivos, copie los datos en servidores recién instalados, establezca ACL mediante usuarios y grupos en el nuevo bosque y, a continuación, cree servidores de impresión de manera similar.  
  
5.  No permita la instalación de sistemas operativos o aplicaciones heredados en el bosque original. Si una aplicación no se puede actualizar y instalar de nuevo, déjela en el bosque heredado y considere la destrucción creativa para reemplazar la funcionalidad de la aplicación.  
  
### <a name="defining-a-limited-risk-based-migration-plan"></a>Definición de un plan de migración limitado y basado en riesgos  
La creación de un plan de migración limitado y basado en riesgos simplemente significa que, al decidir qué usuarios, aplicaciones y datos migrar a su bosque original, debe identificar los objetivos de migración en función del grado de riesgo al que se expone su organización si uno de los usuarios o sistemas están en peligro. Los usuarios VIP cuyas cuentas tengan más probabilidades de ser objetivo de los atacantes deben estar alojadas en el bosque original. Las aplicaciones que proporcionan funciones empresariales esenciales deben instalarse en servidores recién creados en el bosque original, y los datos altamente confidenciales se deben migrar a servidores protegidos en el bosque original.  
  
Si aún no tiene una imagen clara de la mayoría de los usuarios, sistemas, aplicaciones y datos críticos para la empresa en su entorno de Active Directory, trabaje con las unidades de negocio para identificarlas. Se debe identificar cualquier aplicación necesaria para que funcione la empresa, como si se hubiera almacenado algún servidor en el que se ejecuten aplicaciones críticas o datos críticos. Mediante la identificación de los usuarios y recursos necesarios para que la organización siga funcionando, debe crear una colección de activos con prioridades de forma natural en la que centrar los esfuerzos.  
  
### <a name="leveraging-nonmigratory-migrations"></a>Aprovechamiento de migraciones "no migratorias"  
Si sabe que su entorno se ha puesto en peligro, sospecha que se ha puesto en peligro o simplemente prefiere no migrar los datos y objetos heredados de una instalación heredada de Active Directory a uno nuevo, tenga en cuenta los enfoques de migración que técnicamente no "migrar" objetos.  
  
### <a name="user-accounts"></a>Cuentas de usuario  
En una migración de Active Directory tradicional de un bosque a otro, el atributo SIDHistory (historial SID) de los objetos de usuario se usa para almacenar el SID de los usuarios y los SID de los grupos a los que pertenecen los usuarios en el bosque heredado. Si las cuentas de usuario se migran a un nuevo bosque y tienen acceso a los recursos del bosque heredado, los SID del historial de SID se usan para crear un token de acceso que permita a los usuarios tener acceso a los recursos a los que tenían acceso antes de migrar las cuentas.  
  
Sin embargo, mantener el historial de SID ha demostrado un problema en algunos entornos, ya que el rellenado de los tokens de acceso de los usuarios con SID actuales e históricos puede producir una recarga de tokens. La saturación de tokens es un problema en el que el número de SID que debe almacenarse en el token de acceso de un usuario usa o supera la cantidad de espacio disponible en el token.  
  
Aunque los tamaños de los tokens se pueden aumentar hasta una extensión limitada, la solución final al aumento de los tokens es reducir el número de SID asociados a las cuentas de usuario, ya sea mediante la racionalización de la pertenencia a grupos, la eliminación del historial de SID o una combinación de ambos. Para obtener más información acerca de la saturación de tokens, consulte [MaxTokenSize y el incremento de los tokens de Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
En lugar de migrar a los usuarios desde un entorno heredado (especialmente uno en el que se puedan poner en peligro las pertenencias a grupos y los historiales de SID) mediante el historial de SID, considere la posibilidad de aprovechar las aplicaciones de Metadirectorios para "migrar" usuarios, sin tener que llevar historiales de SID en el nuevo bosque. Cuando se crean cuentas de usuario en el nuevo bosque, puede usar una aplicación de metadirectorio para asignar las cuentas a sus cuentas correspondientes en el bosque heredado.  
  
Para proporcionar a las nuevas cuentas de usuario acceso a los recursos del bosque heredado, puede usar las herramientas de metadirectorio para identificar los grupos de recursos a los que se concedió acceso a las cuentas heredadas de los usuarios y, a continuación, agregar las nuevas cuentas de los usuarios a esos grupos. En función de la estrategia de grupo del bosque heredado, puede que tenga que crear grupos locales de dominio para el acceso a los recursos o convertir los grupos existentes en grupos locales de dominio para permitir que se agreguen las nuevas cuentas a los grupos de recursos. Al centrarse primero en las aplicaciones y los datos más importantes y migrarlos al nuevo entorno (con o sin historial de SID), puede limitar la cantidad de esfuerzo dedicado en el entorno heredado.  
  

  
### <a name="servers-and-workstations"></a>Servidores y estaciones de trabajo  
En una migración tradicional de un bosque Active Directory a otro, la migración de equipos suele ser relativamente simple en comparación con la migración de usuarios, grupos y aplicaciones. En función del rol de equipo, la migración a un nuevo bosque puede ser tan simple como separar un dominio antiguo y unirse a uno nuevo. Sin embargo, la migración de cuentas de equipo intactas en un bosque puro anula el propósito de crear un nuevo entorno. En lugar de migrar cuentas de equipo (potencialmente en peligro, mal configuradas o no actualizadas) a un nuevo bosque, debe instalar los servidores y las estaciones de trabajo en el nuevo entorno. Puede migrar datos de sistemas del bosque heredado a sistemas del bosque original, pero no a los sistemas que hospedan los datos.  
  
### <a name="applications"></a>Aplicaciones  

Las aplicaciones pueden presentar el mayor desafío en cualquier migración de un bosque a otro, pero en el caso de una migración "no migratoria", uno de los principios más básicos que debe aplicar es que las aplicaciones del bosque original deben estar actualizadas. compatible y instalado de nuevo. Los datos se pueden migrar desde instancias de aplicación en el bosque anterior siempre que sea posible. En situaciones en las que una aplicación no se puede "volver a crear" en el bosque original, debería considerar métodos como la destrucción creativa o el aislamiento de aplicaciones heredadas, como se describe en la sección siguiente.  
  
### <a name="implementing-creative-destruction"></a>Implementación de la destrucción creativa  
La destrucción creativa es un término económico que describe el desarrollo económico creado por la destrucción de un pedido anterior. En los últimos años, el término se ha aplicado a la tecnología de la información. Normalmente, hace referencia a los mecanismos por los que se elimina la infraestructura antigua, no mediante su actualización, sino que se reemplaza por una nueva. 2011 [Gartner Symposium ITXPO](http://www.gartner.com/technology/symposium/orlando/) para cios y ejecutivos senior de ti presentaron la destrucción creativa como uno de sus temas clave para reducir costos y aumentar la eficacia. Las mejoras en la seguridad son posibles como un mejor crecimiento natural del proceso.  

Por ejemplo, una organización puede estar formada por varias unidades de negocio que usan una aplicación diferente que realiza una funcionalidad similar, con distintos grados de modernización y soporte técnico del proveedor. Históricamente, podría ser responsable de mantener la aplicación de cada unidad de negocio por separado, y los esfuerzos de consolidación consistirían en averiguar qué aplicación ofrecía la mejor funcionalidad y, a continuación, migrar los datos a ese aplicación de las demás.  
  
En la destrucción creativa, en lugar de mantener aplicaciones antiguas o no actualizadas, se implementan aplicaciones completamente nuevas para reemplazar el antiguo, se migran los datos a las aplicaciones nuevas y se descargan las aplicaciones antiguas y los sistemas en los que se ejecutan. En algunos casos, puede implementar la destrucción creativa de aplicaciones heredadas mediante la implementación de una nueva aplicación en su propia infraestructura, pero siempre que sea posible, debería considerar la posibilidad de migrar la aplicación a una solución basada en la nube en su lugar.  
  
Mediante la implementación de aplicaciones basadas en la nube para reemplazar las aplicaciones internas heredadas, no solo se reducen los costos y los esfuerzos de mantenimiento, sino que se reduce la superficie expuesta a ataques de la organización, ya que se eliminan los sistemas heredados y las aplicaciones que presentan vulnerabilidades. para que los atacantes lo aprovechen. Este enfoque proporciona una manera más rápida para que una organización obtenga la funcionalidad deseada, al mismo tiempo que elimina los destinos heredados de la infraestructura. Aunque el principio de la destrucción creativa no se aplica a todos los recursos de ti, proporciona una opción a menudo viable para eliminar sistemas y aplicaciones heredados al mismo tiempo que se implementan aplicaciones sólidas y seguras basadas en la nube.  
  
### <a name="isolating-legacy-systems-and-applications"></a>Aislar sistemas y aplicaciones heredados  
Un análisis natural de la migración de usuarios y sistemas críticos para la empresa a un entorno puro y seguro es que el bosque heredado contendrá información y sistemas menos valiosos. Aunque los sistemas heredados y las aplicaciones que permanecen en el entorno menos seguro pueden presentar riesgos elevados de riesgo, también representan una gravedad reducida de riesgo. Al rehoming y modernizar sus activos empresariales críticos, puede centrarse en la implementación de la administración y supervisión eficaces sin necesidad de acomodar la configuración y los protocolos heredados.  
  
Cuando haya realojado los datos críticos en un bosque original, puede evaluar las opciones para aislar aún más sistemas y aplicaciones heredados en el bosque de AD DS principal. Aunque podría implementar la destrucción creativa para reemplazar una aplicación y los servidores en los que se ejecuta, en otros casos podría considerar el aislamiento adicional de los sistemas y aplicaciones menos seguros. Por ejemplo, una aplicación que usa un puñado de usuarios, pero que requiere credenciales heredadas como los hashes de LAN Manager, se puede migrar a un dominio pequeño que cree para admitir sistemas para los que no tiene opciones de reemplazo.  
  
Al quitar estos sistemas de dominios donde se forzó la implementación de la configuración heredada, puede aumentar la seguridad de los dominios si los configura para admitir solo los sistemas operativos y las aplicaciones actuales. Aunque es preferible retirar los sistemas y las aplicaciones heredados siempre que sea posible. Si la retirada no es factible para un pequeño segmento de la población heredada, la segregación en un dominio (o bosque) independiente permite realizar mejoras incrementales en el resto de la instalación heredada.  
  
### <a name="simplifying-security-for-end-users"></a>Simplificación de la seguridad para los usuarios finales  
En la mayoría de las organizaciones, los usuarios que tienen acceso a la información más confidencial debido a la naturaleza de sus roles en la organización suelen tener la menor cantidad de tiempo para dedicarse a aprender restricciones y controles de acceso complejos. Aunque debe tener un completo programa de aprendizaje de seguridad para todos los usuarios de su organización, también debe centrarse en facilitar la seguridad para que sea más fácil de usar como sea posible mediante la implementación de controles que sean transparentes y simplifiquen los principios en los que los usuarios cumplir.  
  
Por ejemplo, puede definir una directiva en la que los ejecutivos y otras VIP deban usar estaciones de trabajo seguras para acceder a datos confidenciales y sistemas, lo que les permite usar sus otros dispositivos para acceder a datos menos confidenciales. Este es un principio sencillo que los usuarios deben recordar, pero puede implementar una serie de controles de back-end para ayudar a aplicar el enfoque.  

Puede usar el [mecanismo de autenticación](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) para permitir que los usuarios obtengan acceso a datos confidenciales solo si han iniciado sesión en sus sistemas seguros con sus tarjetas inteligentes y pueden usar restricciones de derechos de usuario y IPSec para controlar los sistemas desde los que pueden Conéctese a los repositorios de datos confidenciales. Puede usar el [Kit de herramientas de clasificación de datos de Microsoft](https://www.microsoft.com/download/details.aspx?id=27123) para crear una sólida infraestructura de clasificación de archivos, y puede implementar [Access Control dinámicas](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) para restringir el acceso a los datos en función de las características de un intento de acceso, la traducción. reglas de negocios en controles técnicos.  
  
Desde la perspectiva del usuario, el acceso a datos confidenciales desde un sistema protegido "simplemente funciona" y al intentar hacerlo desde un sistema no seguro "no solo". Sin embargo, desde el punto de vista de la supervisión y la administración de su entorno, está ayudando a crear patrones identificables en la forma en que los usuarios acceden a los sistemas y datos confidenciales, lo que facilita la detección de intentos de acceso anómalos.  
  
En entornos en los que la resistencia del usuario a las contraseñas largas y complejas ha dado como resultado directivas de contraseña insuficientes, especialmente para los usuarios VIP, considere métodos alternativos para la autenticación, ya sea a través de tarjetas inteligentes (que tienen varios factores de forma y con características adicionales para reforzar la autenticación), controles biométricos, como lectores de deslizamiento de dedo, o incluso datos de autenticación protegidos por chips del módulo de plataforma segura (TPM) en los equipos de los usuarios. Aunque la autenticación multifactor no evita los ataques de robo de credenciales si un equipo ya está en peligro, al ofrecer a los usuarios controles de autenticación fáciles de usar, puede asignar contraseñas más sólidas a las cuentas de usuarios para los que el usuario tradicional los controles de nombre y contraseña son difíciles de manejar.  
