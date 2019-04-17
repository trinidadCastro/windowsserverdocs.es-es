---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planificación de compromiso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7cb87e6b9d1ace15050dcbff8b3c6d864c2a18f0
ms.sourcegitcommit: f2e98f8b7828730b83a3cc8f0e33d4e4db1b16e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2018
---
# Planificación de compromiso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legislación número uno: Nadie cree que algo malo puede ocurrir en ellos, hasta que lo haga.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Planes de recuperación de desastres en muchas organizaciones centran en la recuperación de desastres regionales o errores como resultado de pérdida de servicios informáticos. Sin embargo, cuando se trabaja con los clientes en peligro, muchas veces vemos que la recuperación de compromiso intencionado esté ausente en sus planes de recuperación de desastres. Esto ocurre especialmente cuando el compromiso da robo de propiedad intelectual o destrucción intencionada que aprovecha (como los límites lógicos (por ejemplo, destrucción de todos los dominios de Active Directory o todos los servidores) en lugar de límites físicos destrucción de un centro de datos). Aunque una organización tenga planes de respuesta a incidencias que definen actividades iniciales que se ejecuta cuando se detecta un compromiso, estos planes a menudo omiten los pasos para recuperar de peligro que afecta a toda la infraestructura.  
  
Dado que Active Directory proporciona enriquecidas capacidades de administración de identidades y acceso de usuarios, servidores, estaciones de trabajo y aplicaciones, siempre es objetivo de intrusos. Si un intruso obtenga acceso amplios privilegios a un controlador de dominio o un dominio de Active Directory, que access puede aprovecharse para tener acceso, controlar o incluso destroy todo el bosque de Active Directory.  
  
Este documento describe algunos de los ataques más comunes de Windows y Active Directory y debe medidas puede implementar para reducir la superficie de ataque, pero la única forma para recuperar en caso de peligro completo de Active Directory prepararse para el compromiso antes de que suceda. En esta sección se centra menos en detalles de implementación técnica de las secciones anteriores de este documento y, a continuación, más recomendaciones de alto nivel que puede usar para crear un enfoque integral completo para proteger y administrar su organización del crítico empresa y activos de TI.  
  
Si nunca ha ha ataque su infraestructura, ha se resistió a intentar violaciones o ha succumbed a ataques y ha comprometido totalmente, debe planear la realidad inevitable repetidamente ataque. No es posible evitar ataques, pero es realmente posible evitar infracción significativa o compromiso por mayor. Todas las organizaciones estrechamente deben evaluar sus programas de administración de riesgos existente y realice los ajustes necesarios para ayudar a reducir su nivel de vulnerabilidad general haciendo inversiones equilibradas en prevención, detección, contenido y recuperación.  
  
Para crear una defensa eficaz mientras ofrece servicios a los usuarios y empresas que dependen de la infraestructura y las aplicaciones, puede necesita tener en cuenta nuevas formas de evitar, detectar y contienen compromiso en su entorno y recuperar después de el compromiso. Los enfoques y las recomendaciones de este documento no pueden ayudar reparar una instalación de Active Directory comprometida, pero puede ayudar a proteger su uno siguiente.  
  
Recomendaciones para recuperar un bosque de Active Directory se presentan en [Windows Server 2012: planear la recuperación de Active Directory del bosque](https://www.microsoft.com/download/details.aspx?id=16506). Es posible que pueda impedir que el nuevo entorno completamente se vean comprometidos, pero incluso si no puede, tendrá herramientas para recuperar y recuperar el control de su entorno.  
  
## Replanteamiento el enfoque  
*Legislación número ocho: la dificultad de defensa de una red es directamente proporcional a su complejidad.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Es generalmente aceptado que si un intruso ha obtenido sistema, administrador, raíz o acceso equivalente a un equipo, independientemente del sistema operativo, ese equipo ya no se puede considerar fidedigna, independientemente de cuántos esfuerzos realizados para "limpiar" el sistema. Active Directory no es diferente. Si un intruso ha obtenido acceso con privilegios a un controlador de dominio o una cuenta con privilegios elevados en Active Directory, a menos que tenga un registro de todas las modificaciones el intruso consigue o una copia de seguridad buena conocida, nunca puede restaurar el directorio a una por completo estado de confianza.  
  
Cuando un servidor miembro o una estación de trabajo se ha resultado comprometida y modificado por un intruso, el equipo ya no es fidedigna pero adyacentes servidores absoluta y arecompromise de estaciones de trabajo de un equipo no implica que todos los equipos son comprometidos.  
  
Sin embargo, en un dominio de Active Directory, todos los controladores de dominio alojar réplicas de la misma base de datos de AD DS. Si un controlador de dominio está comprometido y un intruso modifica la base de datos de AD DS, las modificaciones replicarán en cada controlador del dominio en el dominio y, dependiendo de la partición en la que se realizan las modificaciones, el bosque. Incluso si reinstalar cada controlador de dominio en el bosque, simplemente va a reinstalar los hosts en el que reside la base de datos de AD DS. Las modificaciones malintencionadas a Active Directory reproducirá en controladores de dominio recién instalados fácilmente que se reproducen en controladores de dominio que se han ejecutado durante años.  
  
Evaluar entornos comprometidos, encontramos con frecuencia que lo que parece ser el primer incumplimiento "evento" realmente activó después de semanas, meses o años incluso después de que los atacantes inicialmente tenían comprometido el entorno. Atacantes suele obtienen las credenciales de cuentas con privilegios elevados largas antes de que se ha detectado un incumplimiento y aprovechan estas cuentas para comprometa el directorio, los controladores de dominio, servidores miembro, estaciones e incluso conectado no sean de Windows sistemas.  
  
Estos resultados son coherentes con varios resultados de Verizon 2012 datos incumplimiento investigaciones informe, que indica que:  
  
-   98 por ciento de violaciones de datos derivó del agentes externos  
  
-   85% de violaciones de datos tomó semanas o más para descubrir  
  
-   porcentaje 92 de incidencias encontrados por terceros, y  
  
-   97% de violaciones estaba evitables aunque simple o intermedio controles.  
  
Un compromiso para el grado en que se ha descrito anteriormente es un modo eficaz irreparable y los consejos estándar "acoplar y volver a crear" cada sistema comprometido simplemente no es factible o que es posible si se ha comprometido o destruye Active Directory. Restaurar incluso en un estado correcto conocido no elimina los errores que permitía el entorno de ha resultado comprometida en primer lugar.  
  
Aunque debe Defender cada aspecto de su infraestructura, un intruso sólo necesita encontrar suficiente defectos en su defensa para acceder a su objetivo deseado. Si su entorno es bastante simple y original y tradicionalmente bien administrado, a continuación, implementar las recomendaciones proporcionadas más adelante en este documento puede ser una propuesta directa.  
  
Sin embargo, hemos encontrado que el anterior, más grandes y más complejos del entorno, más probable es que las recomendaciones de este documento será factible o incluso imposible implementar. Es mucho más difícil proteger una infraestructura después el hecho de que es empezar desde cero y crear un entorno de resistente a ataques y comprometa. Pero, como se indicó anteriormente, no resulta pequeña empresa para volver a generar todo un bosque de Active Directory. Para ello, se recomienda más centrados, enfoque selectivo para proteger su bosques de Active Directory.  
  
En lugar de centrarse en e intentar corregir todas las acciones que están "rotos", considere la posibilidad de un método en el que establecer prioridades en función de es más importante para su negocio y su infraestructura. En lugar de intentar corregir un entorno rellenado con sistemas obsoletos, mal configurados y aplicaciones, considere la posibilidad de crear un nuevo entorno seguro pequeño en la que puede trasladar con seguridad los usuarios, sistemas e información que sean más importantes para su empresa.  
  
En esta sección se describe un enfoque por el que puede crear un bosque de AD DS original que sirve como un "barco vida" o "celda seguro" para su infraestructura de empresa central. Un bosque original es simplemente un recién instalado bosque de Active Directory que normalmente es limitada en tamaño y el ámbito y que se ha creado mediante el uso de sistemas operativos actuales, aplicaciones y con los principios descritos en [reducción de Active Directory ataque Superficie](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Al implementar la configuración recomendada en un bosque recién creado, puede crear una instalación de AD DS integrado conceptos básicos con prácticas y configuración de seguridad y, a continuación, puede reducir los desafíos que acompañan a sistemas heredados y aplicaciones. Mientras instrucciones detalladas para el diseño e implementación de una instalación de AD DS original están fuera del ámbito de este documento, debe seguir algunos principios generales e instrucciones para crear una "celda segura" en el que puede alojar el más importantes activos. Estas instrucciones son las siguientes:  
  
1.  Identificar principios para separar y proteger los activos críticos.  
  
2.  Definir un plan de migración limitada basada en riesgos.  
  
3.  Aproveche las migraciones "nonmigratory" cuando sea necesario.  
  
4.  Implementar "destrucción creative".  
  
5.  Aislar aplicaciones y sistemas heredados.  
  
6.  Simplificar la seguridad para los usuarios finales.  
  
### Identificar principios para separar y proteger los activos críticos  

Las características del entorno original que cree a activos críticos de casa pueden variar. Por ejemplo, puede crear un bosque original en el que migrar solo los usuarios VIP y datos confidenciales que pueden tener acceso los usuarios. Puede crear un bosque original en el que migrar VIP no solo los usuarios, pero que se implementa como un bosque administrativo, implementar los principios descritos en [reducir la superficie de ataque Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) para crear cuentas administrativas seguras y hosts que se pueden usar para administrar su heredados bosques desde el bosque original. Puede implementar un bosque "específico" que aloja cuentas VIP, cuentas con privilegios y sistemas que requieren seguridad adicional, como los servidores de servicios de certificados de Active Directory (AD CS) con el único objetivo de separar a ellos desde menos seguras bosques. Por último, podría implementar un bosque original que se convierta en la ubicación de hecho para todos los nuevos usuarios, sistemas, aplicaciones y datos, lo que le permite finalmente retirar el bosque heredado a través de desgaste.  
  
Independientemente de si su bosque original contiene unos cuantos usuarios y sistemas o forma la base para una migración más agresiva, debe seguir estos principios en la planificación:  
  
1.  Supongamos que se han comprometido su bosques heredados.  
  
2.  No configure un entorno de seguridad para confiar en un bosque heredado, aunque puede configurar un entorno heredado para confiar en un bosque original.  
  
3.  No migrar cuentas de usuario o los grupos de un bosque heredado a un entorno si existe la posibilidad de que las pertenencias a grupos de las cuentas, el historial SID u otros atributos pueden haber sido malintencionada modificados. En su lugar, use "nonmigratory" enfoques para rellenar un bosque original. (Nonmigratory enfoques se describen más adelante en esta sección).  
  
4.  No se migran equipos de bosques heredados a bosques original. Implementar servidores recién instalados en el bosque original, instalar aplicaciones en los servidores recién instalados y migrar datos de la aplicación a los sistemas recién instalados. Servidores de archivos, copiar datos a los servidores recién instalados, establezca las ACL mediante usuarios y grupos en el nuevo bosque y, a continuación, crear servidores de impresión de manera similar.  
  
5.  No permitir la instalación de aplicaciones en el bosque original o sistemas operativos heredados. Si una aplicación no se actualizan y se recién instalada, deje del bosque heredados y considere la posibilidad de creative destrucción para reemplazar la funcionalidad de la aplicación.  
  
### Definición de un Plan de migración limitado, en función del riesgo  
Crear un limitado, plan de migración en función del riesgo simplemente significa que al decidir qué usuarios, aplicaciones y datos migrar a su bosque original, debe identificar los objetivos de la migración según el grado de riesgo a que su organización se expone si uno de los usuarios o sistemas está comprometido. Los usuarios de la dirección VIP cuyas cuentas suelen ser el objetivo de intrusos deben encontrarse en el bosque original. Aplicaciones que proporcionan funciones de negocio fundamental deben instalarse en servidores recién creados en el bosque original y, a continuación, se debe mover información delicada a protegen servidores del bosque original.  
  
Si no dispone de una imagen clara de los usuarios empresariales importantes, sistemas, aplicaciones y datos en su entorno de Active Directory, trabajan con las unidades de negocio identificarlos. Cualquier aplicación necesaria para que el negocio funcionando debe identificarse, así como todos los servidores que ejecutan aplicaciones críticas o se almacenan datos importantes. Mediante la identificación de los usuarios y recursos necesarios para su organización siga funcionando, crear una colección naturalmente prioridad de activos en el que centrar sus esfuerzos.  
  
### Sacar provecho de las migraciones "Nonmigratory"  
Si sabe que su entorno se ha comprometido, sospecha que se ha comprometido, o simplemente prefiere no migrar datos heredados y objetos de una instalación de Active Directory heredada a una nueva, puede optar por enfoques de migración que técnicamente no hacer objetos "migrar".  
  
### Cuentas de usuario  
En una migración tradicional de Active Directory desde un bosque a otro, el atributo SIDHistory (SIDHistory) en los objetos de usuario se usa para almacenar los SID de los usuarios y los SID de los grupos que los usuarios eran miembros del bosque heredados. Si se migran cuentas de usuarios a un nuevo bosque y acceden a los recursos del bosque heredado, los SID en el historial de SID se usan para crear un token de acceso que permite a los usuarios acceso a los recursos a la que tenían acceso antes de que se han migrado las cuentas.  
  
Mantener el historial de SID, sin embargo, ha comprobado problemático en algunos entornos porque rellenar tokens de acceso de los usuarios con SID histórica y actual podría aumento del token. Aumento del token es un problema en el que se usa el número de SID que deben estar almacenados en el token de acceso de un usuario o supera la cantidad de espacio disponible en el símbolo.  
  
Aunque puede aumentar el tamaño de token hasta cierto punto, la solución final a aumento del token es reducir el número de SID asociados con cuentas de usuario, ya sea por racionalización pertenencias a grupos, eliminar el historial SID o una combinación de ambos. Para obtener más información sobre el aumento del token, consulte [MaxTokenSize y engordar Token de Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
En lugar de migrar usuarios desde un entorno heredado (especialmente uno en el que las pertenencias a grupos y SID pueden verse comprometidos historiales) con el historial de SID, considere la posibilidad de aprovechar aplicaciones metadirectorios "migrar" usuarios, sin ejecutar historiales en el nuevo bosque. Cuando se crean cuentas de usuario en el nuevo bosque, puede usar una aplicación de metadirectorio para asignar las cuentas a sus cuentas correspondientes del bosque heredados.  
  
Para proporcionar el acceso de cuentas de usuario nuevo a los recursos del bosque heredados, puede usar las herramientas de metadirectorio para identificar los grupos de recursos en las cuentas antiguas de los usuarios se le ha concedido acceso y, a continuación, agregue nuevas cuentas de los usuarios a los grupos. Dependiendo de la estrategia de grupo del bosque heredados, debe crear grupos locales de dominio para el acceso a los recursos o convertir a los grupos existentes en grupos locales de dominio para permitir que las nuevas cuentas de agregarse a grupos de recursos. Al enfocarse primero en las aplicaciones más importantes y los datos y migrarlos al nuevo entorno (con o sin SIDHistory), puede limitar la cantidad de esfuerzo realizado en el entorno heredado.  
  

  
### Los servidores y estaciones de trabajo  
En una migración tradicional desde un Active Directory bosque a otro, migrar equipos suele ser relativamente simple en comparación con la migración de usuarios, grupos y aplicaciones. Dependiendo de la función del equipo, la migración a un nuevo bosque puede ser tan sencilla como quitando un dominio antiguo y unirse a una nueva. Migrar sin embargo, cuentas de equipo intactas en una derrota bosque original el propósito de la creación de un entorno actualizado. En lugar de migrar (posiblemente comprometida, mal configurado u obsoleta) cuentas de equipo a un nuevo bosque, debe instalar recién servidores y estaciones de trabajo en el nuevo entorno. Puede migrar datos de sistemas del bosque heredados a sistemas del bosque original, pero no los sistemas casa de los datos.  
  
### Aplicaciones  

Aplicaciones pueden presentar el desafío más importante en cualquier migración desde un bosque a otro, pero en el caso de una migración "nonmigratory", uno de los principios más básicos que debe aplicarse es que las aplicaciones en el bosque original deben ser actuales, admite y recién instalado. Se pueden migrar datos de instancias de la aplicación del bosque antiguo siempre que sea posible. En las situaciones en que una aplicación no se "reconstruirse" en el bosque original, debe tener en cuenta enfoques como su creatividad destrucción o aislamiento de aplicaciones heredadas como se describe en la sección siguiente.  
  
### Implementación de Creative destrucción  
Destrucción Creative es un término de ahorro que describa el desarrollo económico creado por la destrucción de un pedido anterior. En los últimos años, se ha aplicado el término a la tecnología de información. Normalmente se refiere a mecanismos por qué infraestructura antigua se elimina, no por actualizarlo, pero reemplazándola por algo totalmente nuevo. El 2011 [Gartner simposio ITXPO](http://www.gartner.com/technology/symposium/orlando/) para los directores informáticos y ejecutivos de TI presenta creative destrucción como uno de sus temas claves para reducir los costos y aumenta la eficiencia. Mejoras en seguridad son posibles como un producto natural del proceso.  

Por ejemplo, una organización puede estar formada por varias unidades de negocio que usan una aplicación diferente que realiza funciones similares, con diversos grados de soporte técnico de modernity y proveedor. Tradicionalmente, TI podría ser responsable de mantener la aplicación de cada unidad de negocio por separado y los esfuerzos de consolidación puede consistir intentar averiguar qué aplicación ofrece la funcionalidad mejor y, a continuación, migrar datos en el que aplicación de los demás.  
  
En su creatividad destrucción, en lugar de mantener aplicaciones obsoletas o redundantes, implementar aplicaciones completamente nuevas para reemplazar al antiguo, migrar datos a las nuevas aplicaciones y retirar el antiguas aplicaciones y sistemas en los que se ejecutan. En algunos casos, puede implementar su creatividad destrucción de aplicaciones heredadas implementar una nueva aplicación en su propia infraestructura, pero siempre que sea posible, considere la posibilidad de llevar la aplicación a una solución basada en la nube en su lugar.  
  
Implementar aplicaciones basadas en la nube para reemplazar las aplicaciones internas heredadas, no solo reducir el esfuerzo de mantenimiento y costos, pero reducir ataque de su organización mediante la eliminación de sistemas heredados y aplicaciones que presentan vulnerabilidad para los atacantes aprovechar. Este enfoque proporciona una manera más rápida de obtener la funcionalidad deseada simultáneamente eliminando heredados destinos en la infraestructura de una organización. Aunque el principio de creative destrucción no se aplica a todos los activos de TI, proporciona una opción para eliminar los sistemas heredados y aplicaciones al implementar simultáneamente aplicaciones sólidas, seguras basada en nube viable a menudo.  
  
### Aislar sistemas heredados y aplicaciones  
Un producto natural de migrar los usuarios empresariales y sistemas de un entorno original, segura es su bosque heredado se contendrá información menos valiosa y sistemas. Aunque los sistemas heredados y aplicaciones que permanecen en el entorno menos seguro pueden presentar elevado riesgo de compromiso, también representar una reducida gravedad del riesgo. Realojar y modernizado los activos empresariales importantes, podrá centrarse en implementar la administración de efectivo y la supervisión mientras no se necesitan acomodar protocolos y configuración heredados.  
  
Cuando se vuelven a ubicar los datos críticos a un bosque original, puede evaluar opciones para aislar aún más los sistemas heredados y aplicaciones en su bosque de AD DS "principal". Aunque podría implementar creative destrucción para reemplazar una aplicación y los servidores en el que se ejecuta, en los demás casos podría considerar aislamiento adicionales de los sistemas y aplicaciones menos seguro. Por ejemplo, una aplicación que se utiliza por unos cuantos usuarios, pero que requiere credenciales heredadas como hash LAN Manager se pueden migrar a un dominio pequeño crea para admitir sistemas para el que tiene no hay opciones de reemplazo.  
  
Al quitar estos sistemas de dominios donde forzado implementación de configuración heredados, posteriormente puede aumentar la seguridad de los dominios configurando para admitir sólo sistemas operativos actuales y las aplicaciones. Aunque es preferible retirar sistemas heredados y aplicaciones siempre que sea posible. Si para retirar simplemente no es factible para un pequeño segmento de la población heredado, separar en un dominio independiente (o bosque) permite realizar mejoras incrementales en el resto de la instalación antigua.  
  
### Simplificar la seguridad para los usuarios finales  
En la mayoría de las organizaciones, los usuarios que tienen acceso a la información más confidencial debido a la naturaleza de sus funciones de la organización a menudo tienen la menor cantidad de tiempo que dedique a los controles y las restricciones de acceso compleja de aprendizaje. Aunque deben tener un programa de educación de seguridad completa para todos los usuarios de su organización, también debe centrarse en realizar seguridad como fáciles de usar como sea posible mediante la implementación de controles que son principios transparentes y simplificar a los usuarios que cumplir.  
  
Por ejemplo, puede definir una directiva en la que los ejecutivos y otro VIP requiere estaciones de trabajo seguras para tener acceso a información confidencial y sistemas, lo que les permite usar sus otros dispositivos para tener acceso a datos menos confidenciales. Este es un principio sencillo recordar para los usuarios, pero puede implementar una serie de controles de back-end para ayudar a exigir el enfoque.  

Puede usar [La comprobación de mecanismo de autenticación](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) para permitir que los usuarios tengan acceso a datos confidenciales solo si ha iniciado sesión en sus sistemas seguros utilizando sus tarjetas inteligentes y puede usar las restricciones de derechos de usuario y IPsec para controlar los sistemas desde la que se pueden conectarse a repositorios de información confidencial. Puede usar el [Kit de herramientas de clasificación de datos de Microsoft](https://www.microsoft.com/download/details.aspx?id=27123) para crear una infraestructura de clasificación de archivo sólido y, a continuación, puede implementar el [Control de acceso dinámico](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) para restringir el acceso a datos en función de las características de un intento de acceso, traducir reglas de negocios en controles técnicos.  
  
Desde la perspectiva del usuario, acceso a los datos confidenciales de un sistema protegido "simplemente funciona" y a continuación, trate de hacerlo desde un sistema no seguro "simplemente no". Sin embargo, desde la perspectiva de supervisión y administración de su entorno, puede ayudar a crear patrones de identificación personal en cómo los usuarios de acceso a datos confidenciales y sistemas facilitará detectar intentos de acceso irregulares.  
  
En entornos en que el usuario ha generado la resistencia a contraseñas largos y complejas en directivas de contraseña suficiente, especialmente para los usuarios de la dirección VIP, considere la posibilidad de enfoques alternativos para la autenticación, a través de las tarjetas inteligentes (que vienen en un número de factores de forma y con funciones adicionales para aumentar la autenticación), controles biométricos como los lectores deslice rápidamente el dedo o datos de autenticación par que están protegidos por confianza chips de módulo de plataforma (TPM) en los equipos de los usuarios. Aunque la autenticación multifactor impedir ataques de robo de credenciales si ya está comprometido un equipo, asigne a los usuarios controles fáciles de usar autenticación, puede asignar contraseñas más sólidas para las cuentas de usuarios de quienes tradicional usuario controles de nombre y la contraseña son difíciles.  
