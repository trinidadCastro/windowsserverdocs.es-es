---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planeación de compromiso
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f6a30c91590667a0894b52ec7c188a43bd5e351
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835576"
---
# <a name="planning-for-compromise"></a>Planeación de compromiso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número uno: Nadie cree que algo malo puede ocurrir, hasta que lo haga.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Los planes de recuperación ante desastres en muchas organizaciones se centran en la recuperación ante desastres regionales o errores que provocan pérdida de servicios informáticos. Sin embargo, cuando se trabaja con los clientes en peligro, se descubrirán que recuperarse de peligro intencionado está ausente en sus planes de recuperación ante desastres. Esto es especialmente cierto cuando da como resultado el riesgo de robo de propiedad intelectual o destrucción intencionada que aprovecha los límites lógicos (por ejemplo, la destrucción de todos los dominios de Active Directory o todos los servidores) en lugar de los límites físicos (como destrucción de un centro de datos). Aunque una organización puede tener planes de respuesta a incidentes que definen actividades iniciales que se realizará cuando se detecta un riesgo, estos planes a menudo omiten los pasos para recuperarse de un compromiso que afecte a toda la infraestructura informática.  
  
Dado que Active Directory proporciona capacidades de administración de identidades y acceso enriquecidas para los usuarios, servidores, estaciones de trabajo y aplicaciones, invariablemente está dirigido por los atacantes. Si un atacante obtiene acceso con privilegios elevados en un controlador de dominio o dominio de Active Directory, que el acceso se puede aprovechar para tener acceso, controlar o incluso destruir todo el bosque de Active Directory.  
  
Este documento se han tratado algunos de los ataques más comunes en Windows y Active Directory y las medidas que puede implementar para reducir la superficie expuesta a ataques, pero la única forma de recuperar en caso de un riesgo importante para Active Directory que se va a ser preparado para el riesgo antes de que suceda. En esta sección se centra menos en los detalles de implementación técnica de las secciones anteriores de este documento, y más recomendaciones de alto nivel que puede usar para crear un enfoque holístico e integral para proteger y administrar su organización del crítico negocio y activos de TI.  
  
Si su infraestructura no es atacada nunca, se mostraron contrarios al intentar infracciones, o tiene succumbed a ataques y ha totalmente en peligro, debe planear la realidad inevitable que será objeto de ataques y otra vez. No es posible evitar ataques, pero es realmente posible evitar infracciones significativa o por mayor riesgo. Todas las organizaciones estrechamente deben evaluar sus programas de administración de riesgos existente y realizar los ajustes necesarios para ayudar a reducir su nivel general de la vulnerabilidad mediante la realización de las inversiones equilibradas de prevención, detección, la contención y recuperación.  
  
Para crear una protección eficaz sin dejar de ofrecer servicios a los usuarios y las empresas que dependen de su infraestructura y aplicaciones, tiene puede que considere la posibilidad de maneras nuevas para evitar, detectar y contener riesgos en su entorno y, a continuación, recuperarse el riesgo. Los enfoques y las recomendaciones de este documento no le ayude reparar una instalación de Active Directory en peligro, pero puede ayudarle a proteger su siguiente uno.  
  
Se presentan recomendaciones para la recuperación de un bosque de Active Directory en [Windows Server 2012: Planear la recuperación de bosques de Active Directory de](https://www.microsoft.com/download/details.aspx?id=16506). Es posible que pueda impedir que el nuevo entorno completamente se vean comprometidos pero, incluso si no es posible, tendrá las herramientas para recuperar y recuperar el control de su entorno.  
  
## <a name="rethinking-the-approach"></a>Replanteamiento el enfoque  
*Ley número ocho: La dificultad de defensa de una red es directamente proporcional a su complejidad.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Es por lo general aceptado que si un atacante haya obtenido de sistema, administrador, raíz o acceso equivalente a un equipo, independientemente del sistema operativo, ese equipo puede ya no se consideren de confianza, independientemente de cuántas esfuerzos se realizan para "limpiar" el sistema. Active Directory no es diferente. Si un atacante haya obtenido acceso con privilegios a un controlador de dominio o una cuenta con privilegios elevados en Active Directory, a menos que tenga un registro de todas las modificaciones hace que el atacante o una copia de seguridad buena conocida, nunca puede restaurar el directorio a una por completo estado de confianza.  
  
Cuando un servidor miembro o una estación de trabajo está en peligro y modificado por un atacante, el equipo ya no es de confianza, pero todavía se pueden confiar vecinos servidores no poner en peligro y estaciones de trabajo. Poner en peligro un equipo no implica que todos los equipos están en peligro.  
  
Sin embargo, en un dominio de Active Directory, todos los controladores de dominio hospedan réplicas de la misma base de datos de AD DS. Si se pone en peligro un controlador de dominio y un atacante modifica la base de datos de AD DS, esas modificaciones replicarán en cada controlador de dominio en el dominio y, en función de la partición en la que se realizan las modificaciones, el bosque. Incluso si reinstala cada controlador de dominio del bosque, simplemente va a reinstalar los hosts en el que reside la base de datos de AD DS. Las modificaciones malintencionadas a Active Directory replicará a los controladores de dominio recién instalada tan fácilmente como replicará a controladores de dominio que se han ejecutado durante años.  
  
En la evaluación de entornos en peligro, normalmente encontramos que lo que parece ser la primera infracción "evento" realmente desencadenó después de semanas, meses o incluso años después de que los atacantes inicialmente habían puesto en peligro el entorno. Los atacantes suele obtienen las credenciales de cuentas con privilegios elevados mucho antes de que se ha detectado una infracción, y aprovechan las cuentas para poner en peligro el directorio, los controladores de dominio, servidores miembro, estaciones de trabajo y conectados incluso que no sean de Windows sistemas.  
  
Estos resultados son coherentes con varios resultados 2012 datos infracción de las investigaciones de informe de Verizon, que indica que:  
  
-   98 por ciento de las infracciones de datos que se derivó del agentes externos  
  
-   85 por ciento de las infracciones de datos tardó semanas o más que descubrir  
  
-   se detectaron el 92% de incidentes por un tercero, y  
  
-   97% de infracciones son evitables aunque simple o intermedio de controles.  
  
Un compromiso para el grado en que se ha descrito anteriormente es irreparable de forma eficaz y el Consejo estándar "simplificar y volver a generar" todos los sistemas en peligro simplemente no son factible o que es posible si se ha puesto en peligro o destruir Active Directory. Incluso restaurar a un estado válido conocido no elimina los errores que pueden estar en peligro en primer lugar el entorno.  
  
Aunque debe Defender cada faceta de la infraestructura, un atacante sólo necesita encontrar suficiente defectos en sus defensas para llegar al objetivo deseado. Si su entorno es relativamente sencilla y puro e históricamente bien administrados, a continuación, implementar las recomendaciones proporcionadas anteriormente en este documento puede ser una propuesta directa.  
  
Sin embargo, hemos encontrado que el anterior, más grandes y más complejos del entorno, más probable es que las recomendaciones de este documento será factible o incluso imposible implementar. Es mucho más difícil de proteger una infraestructura después del hecho de que es empezar de cero y construir un entorno que sea resistente a ataques y poner en peligro. Pero como se indicó anteriormente, no es ninguna empresa pequeña volver a generar todo un bosque de Active Directory. Por estas razones, se recomienda más centrados, específicos sobre cómo proteger los bosques de Active Directory.  
  
En lugar de centrarse en e intenta corregir todos los aspectos que se "interrumpen", considere la posibilidad de un enfoque en el que establecer prioridades en función de lo que es más importante para su negocio y de su infraestructura. En lugar de intentar corregir un entorno con sistemas obsoletos y mal configurados y las aplicaciones, considere la posibilidad de crear un nuevo entorno pequeño y seguro en la que puede portar con seguridad los usuarios, sistemas e información que son más importantes para su negocio.  
  
En esta sección, se describe un enfoque por el que puede crear un bosque de AD DS original que actúa como un "barco vida" o "celda seguro" para la infraestructura de negocio principal. Un bosque original es simplemente un recién instalado bosque de Active Directory que normalmente está limitado en tamaño y ámbito, y que se ha creado mediante el uso de sistemas operativos actuales, las aplicaciones y con los principios descritos en [reducir activo Superficie de ataque del directorio](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Al implementar la configuración recomendada en un bosque recién creado, puede crear una instalación de AD DS que está creada desde cero con una configuración segura y procedimientos recomendados, y puede reducir los desafíos que acompañan a la compatibilidad con sistemas heredados y aplicaciones. Mientras que las instrucciones detalladas para el diseño e implementación de una instalación original de AD DS están fuera del ámbito de este documento, debe seguir algunos principios generales e instrucciones para crear una "celda segura" en la que puede alojar los más críticos activos. Estas instrucciones son las siguientes:  
  
1.  Identifique los principios para separar y proteger los activos críticos.  
  
2.  Definir un plan de migración limitados, basado en riesgos.  
  
3.  Aproveche las migraciones "nonmigratory" cuando sea necesario.  
  
4.  Implementar "destrucción creative".  
  
5.  Aislar las aplicaciones y sistemas heredados.  
  
6.  Simplifique la seguridad para los usuarios finales.  
  
### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificación de los principios para separar y proteger los activos críticos  

Las características del entorno original que cree a recursos críticos de casa pueden variar enormemente. Por ejemplo, puede crear un bosque original en la que migra solo los usuarios de VIP y sólo esos usuarios puedan acceder a información confidencial. Puede crear un bosque original en el que no se migra solo los usuarios VIP, pero que se implementa como un bosque administrativo, implementar los principios descritos en [lo que reduce la superficie de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) crear segura las cuentas administrativas y los hosts que pueden usarse para administrar los bosques heredados desde el bosque original. Podría implementar un bosque "creado" que aloje cuentas, las cuentas con privilegios y sistemas que necesitan una seguridad adicional, como los servidores que ejecutan servicios de certificados de Active Directory (AD CS) con el único objetivo de y los separa de menos seguras de VIP bosques. Por último, podría implementar un bosque original que se convierte en la ubicación de hecho para todos los nuevos usuarios, sistemas, aplicaciones y datos, lo que le permite dar de baja al final del bosque a través de desgaste heredado.  
  
Independientemente de si su bosque original contiene unos cuantos usuarios y los sistemas o lo constituye la base para una migración más agresiva, debe seguir estos principios en la planeación:  
  
1.  Se supone que los bosques heredados que se haya comprometido.  
  
2.  No configure un entorno puro para confiar en un bosque heredado, aunque puede configurar un entorno heredado para confiar en un bosque original.  
  
3.  No migre las cuentas de usuario o grupos de un bosque heredado a un entorno puro si existe una posibilidad de que las pertenencias a grupos de cuentas, el historial de SID u otros atributos que se han modificado malintencionadamente. En su lugar, use "nonmigratory" enfoques para rellenar un bosque original. (Nonmigratory enfoques se describen más adelante en esta sección).  
  
4.  No migre los equipos de bosques heredados a bosques puro. Implementar servidores recién instalados en el bosque original, instalar aplicaciones en los servidores recién instalados y migrar datos de la aplicación a los sistemas recién instalados. Servidores de archivos, copiar los datos a los servidores recién instalados, establezco las ACL mediante usuarios y grupos en el nuevo bosque y, a continuación, crear servidores de impresión de forma similar.  
  
5.  No permiten la instalación de los sistemas operativos heredados o aplicaciones en el bosque original. Si una aplicación no se actualiza y recién instalada, déjelo en el bosque heredado y considere la posibilidad de destrucción creativa para reemplazar la funcionalidad de la aplicación.  
  
### <a name="defining-a-limited-risk-based-migration-plan"></a>Definir un Plan de migración limitada, en función del riesgo  
Crear un limitado, plan de migración basado en riesgos simplemente quiere decir que la hora de decidir qué usuarios, aplicaciones y datos para migrar a su bosque original, debe identificar los destinos de migración en función del grado de riesgo a la que su organización se expone si uno de los usuarios o sistemas se ve comprometida. Usuarios VIP cuyas cuentas suelen ser objetivo de los atacantes deben alojarse en el bosque original. Las aplicaciones que proporcionan las funciones empresariales esenciales que deben instalarse en servidores recién compilados en el bosque original y se deben mover datos altamente confidenciales a protegen los servidores en el bosque original.  
  
Si no dispone de una imagen clara de los usuarios empresariales más críticas, sistemas, aplicaciones y datos en su entorno de Active Directory, trabajan con unidades de negocio para identificarlos. Cualquier aplicación necesaria para que la empresa opere debe identificarse, igual que cualquier servidor en el que ejecutan aplicaciones críticas o datos críticos están almacenados. Mediante la identificación de los usuarios y recursos que son necesarios para su organización a seguir funcionando, cree una colección de forma natural con prioridad de activos en el que se va a centrar sus esfuerzos.  
  
### <a name="leveraging-nonmigratory-migrations"></a>Aprovechamiento de las migraciones "Nonmigratory"  
Si sabe que su entorno está en peligro, sospecha que se ha puesto en peligro o simplemente prefiere no migrar objetos y datos heredados de una instalación de Active Directory heredada a una nueva, considere la posibilidad de enfoques de migración que no es técnicamente objetos "migrar".  
  
### <a name="user-accounts"></a>Cuentas de usuario  
En una migración tradicional de Active Directory desde un bosque a otro, el atributo SIDHistory (historial de SID) en objetos de usuario se utiliza para almacenar los SID de los usuarios y los SID de grupos que los usuarios eran miembros de en el bosque heredado. Si se migran las cuentas de usuarios a un nuevo bosque, y tienen acceso a recursos en el bosque heredado, los SID en el historial de SID se utilizan para crear un token de acceso que permite a los usuarios tengan acceso a la que tenían acceso antes de que se han migrado las cuentas de recursos.  
  
Mantener el historial de SID, sin embargo, ha demostrado ser problemático en algunos entornos porque rellenar los tokens de acceso de los usuarios con SID actual e histórico puede dar lugar a una congestión de token. Sobredimensionamiento de token es un problema en el que se usa el número de SID que deben almacenarse en el token de acceso de un usuario o supera la cantidad de espacio disponible en el token.  
  
Aunque los tamaños de token se pueden aumentar hasta cierto punto, la solución definitiva sobredimensionamiento de token es reducir el número de SID asociados con cuentas de usuario, ya sea por la racionalización de pertenencias a grupos, lo que elimina el historial de SID o una combinación de ambos. Para obtener más información sobre el sobredimensionamiento del token, consulte [MaxTokenSize y provocaría el sobredimensionamiento Token Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
En lugar de migrar usuarios desde un entorno heredado (especialmente uno en el que las pertenencias a grupos y el SID pueden suponer un riesgo historiales) con el historial de SID, considere la posibilidad de aprovechar aplicaciones metadirectorios "migración" de los usuarios, sin tener que llevar los historiales de SID en el nuevo bosque. Cuando se crean las cuentas de usuario en el nuevo bosque, puede usar una aplicación de metadirectorio para asignar las cuentas a sus cuentas correspondientes en el bosque heredado.  
  
Para proporcionar el acceso de cuentas de usuario nuevo a los recursos en el bosque heredado, puede usar las herramientas de metadirectorio para identificar los grupos de recursos en la que cuentas heredados de los usuarios se le permitiera el acceso y, a continuación, agregar nuevas cuentas de los usuarios a esos grupos. Dependiendo de la estrategia de grupo en el bosque heredada, debe crear grupos locales de dominio para el acceso a los recursos ni convertir a grupos existentes en los grupos locales de dominio para permitir que las cuentas nuevas que se agregarán a los grupos de recursos. Al enfocarnos principalmente en las aplicaciones más importantes y los datos y migrarlos al nuevo entorno (con o sin el historial de SID), puede limitar la cantidad de esfuerzo realizado en el entorno heredado.  
  

  
### <a name="servers-and-workstations"></a>Los servidores y estaciones de trabajo  
En una migración tradicional de Active Directory un bosque a otros, migrar equipos a menudo es relativamente sencillo en comparación con la migración de usuarios, grupos y aplicaciones. Dependiendo de la función de equipo, puede ser tan simple como separar un dominio antiguo y la unión de una nueva migración a un nuevo bosque. Migrar sin embargo, las cuentas de equipo intactas en una derrota bosque original la finalidad de crear un entorno nuevo. En lugar de migrar (potencialmente en peligro, mal configurado u obsoleto) cuentas de equipo a un nuevo bosque, debe instalar recién servidores y estaciones de trabajo en el nuevo entorno. Puede migrar datos desde sistemas en el bosque heredado para los sistemas en el bosque original, pero no a los sistemas que contienen los datos.  
  
### <a name="applications"></a>Aplicaciones  

Las aplicaciones pueden presentar el desafío más importante en cualquier migración desde un bosque a otro, pero en el caso de una migración "nonmigratory", uno de los principios más básicos que se debe aplicar es que las aplicaciones en el bosque original deben ser actuales, admite y recién instalado. Datos se pueden migrar desde instancias de la aplicación en el bosque anterior siempre que sea posible. En situaciones en que una aplicación no puede ser "recreada" en el bosque original, debe considerar otros enfoques como el aislamiento de aplicaciones heredadas o destrucción creative tal como se describe en la sección siguiente.  
  
### <a name="implementing-creative-destruction"></a>Implementación de destrucción creativa  
Destrucción de Creative es un término de ahorro que describe su desarrollo económico creado por la destrucción de un pedido anterior. En los últimos años, el término se ha aplicado a la tecnología de la información. Suele hacer referencia a los mecanismos de por qué infraestructura antigua se elimina, no con una actualización, pero al sustituirla por algo completamente nuevo. El 2011 [Gartner Symposium ITXPO](http://www.gartner.com/technology/symposium/orlando/) para CIO y ejecutivos de TI presentan destrucción creativa como uno de sus temas claves para reducir los costos y aumenta la eficacia. Mejoras en seguridad son posibles como un producto natural del proceso.  

Por ejemplo, una organización puede estar compuesta de varias unidades de negocio que usan una aplicación diferente que realiza una funcionalidad similar, con diferentes grados de compatibilidad modernity y proveedor. Históricamente, TI puede ser responsable de mantener la aplicación de cada unidad de negocio por separado, y los esfuerzos de consolidación consistiría en intentar averiguar qué aplicación ofrecía una mejor funcionalidad y, a continuación, migrar datos en el que aplicación de los demás.  
  
En creative destrucción, en lugar de mantener aplicaciones obsoletas o redundantes, implemente aplicaciones completamente nuevas para reemplazar a la antigua, migrar datos a las nuevas aplicaciones y retirar las aplicaciones antiguas y los sistemas donde se ejecutan. En algunos casos, puede implementar creative destrucción de las aplicaciones heredadas mediante la implementación de una nueva aplicación en su propia infraestructura, pero siempre que sea posible, considere la posibilidad de migrar la aplicación a una solución basada en la nube en su lugar.  
  
Si implementa aplicaciones basadas en la nube para reemplazar las aplicaciones internas heredadas, no solo reducirá los esfuerzos de mantenimiento y los costos, pero reducir la superficie expuesta a ataques de su organización mediante la eliminación de los sistemas heredados y aplicaciones que presentan las vulnerabilidades para los atacantes aprovechar. Este enfoque proporciona una manera más rápida de una organización obtener la funcionalidad deseada mientras se elimina al mismo tiempo heredados destinos en la infraestructura. Aunque el principio de destrucción creative no se aplica a todos los recursos de TI, proporciona una opción viable a menudo para eliminar los sistemas heredados y aplicaciones al implementar al mismo tiempo aplicaciones robustas, seguras y basado en la nube.  
  
### <a name="isolating-legacy-systems-and-applications"></a>Aislar aplicaciones y sistemas heredados  
Un producto natural de la migración de sus usuarios empresariales y sistemas a un entorno seguro y puro es que el bosque heredado contendrá menos valiosa información y sistemas. Aunque los sistemas heredados y aplicaciones que permanecen en el entorno menos seguro, pueden presentar un riesgo elevado de riesgo, también representan una gravedad del riesgo reducida. Realojamiento y modernizar sus activos críticos del negocio, puede centrarse en la administración eficaz de implementar y supervisar al no necesitar dar cabida a protocolos y la configuración heredada.  
  
Cuando se vuelven a ubicar sus datos críticos a un bosque original, puede evaluar las opciones para aislar aún más los sistemas heredados y aplicaciones en su bosque de AD DS "principal". Aunque podría implementar destrucción creativa para reemplazar una aplicación y los servidores en el que se ejecuta, en otros casos es posible que considere aislamiento adicional de los sistemas y aplicaciones menos seguras. Por ejemplo, una aplicación que usa unos cuantos usuarios, sino que requiere credenciales heredadas como valores hash de LAN Manager se pueden migrar a un dominio pequeño cree para admitir los sistemas que no tengan ninguna opción de reemplazo.  
  
Mediante la eliminación de estos sistemas de dominios donde fuerza la implementación de la configuración heredada, posteriormente puede aumentar la seguridad de los dominios mediante la configuración para permitir solo aplicaciones y sistemas operativos actuales. Aunque es preferible para retirar los sistemas heredados y las aplicaciones siempre que sea posible. Si retirada simplemente no es factible para un segmento pequeño de su población heredada, y se separa en un dominio independiente (o bosque) le permite realizar mejoras incrementales en el resto de la instalación antigua.  
  
### <a name="simplifying-security-for-end-users"></a>Simplificación de seguridad para los usuarios finales  
En la mayoría de las organizaciones, los usuarios que tienen acceso a la información más confidencial debido a la naturaleza de sus roles de la organización a menudo tienen la menor cantidad de tiempo para dedicar a controles y restricciones de acceso complejas de aprendizaje. Aunque debe tener un programa de formación de seguridad completa para todos los usuarios de su organización, también debe centrarse en hacer la seguridad como fácil de usar como sea posible mediante la implementación de controles que son transparentes y simplificar los principios para que los usuarios respetar.  
  
Por ejemplo, puede definir una directiva en el que los ejecutivos y otras direcciones IP virtuales deben usar estaciones de trabajo seguras para tener acceso a datos confidenciales y sistemas, le permite usar sus otros dispositivos para tener acceso a datos de poca importancia. Este es un principio sencillo recordar para los usuarios, pero se puede implementar una serie de controles de back-end para ayudar a aplicar el enfoque.  

Puede usar [comprobación del mecanismo de autenticación](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) para permitir que los usuarios tengan acceso a datos confidenciales solo si se ha iniciado sesión en sus sistemas seguros mediante sus tarjetas inteligentes y puede utilizar IPsec y las restricciones de control de derechos de usuario del sistemas desde la que puede conectarse a repositorios de datos confidenciales. Puede usar el [Microsoft Data Classification Toolkit](https://www.microsoft.com/download/details.aspx?id=27123) crear una infraestructura de clasificación de archivos sólido y se puede implementar [Control de acceso dinámico](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) para restringir el acceso a datos basados en características de un intento de acceso, traducir las reglas de negocios a los controles técnicos.  
  
Desde la perspectiva del usuario, obtener acceso a datos confidenciales de un sistema protegido "simplemente funciona" e intentar hacerlo desde el sistema no seguro "simplemente no". Sin embargo, desde la perspectiva de la supervisión y administración de su entorno, puede ayudar a crear patrones de identificación en cómo los usuarios tener acceso a datos confidenciales y sistemas, lo que facilita detectar intentos de acceso anómalos.  
  
En entornos en que el usuario ha producido resistencia a las contraseñas largas y complejas en las directivas de contraseña suficiente, especialmente para los usuarios de VIP, considere la posibilidad de enfoques alternativos para la autenticación, a través de las tarjetas inteligentes (incluidas en una serie de factores de formulario y con características adicionales para reforzar la autenticación), controles biométricos como lectores de deslizar rápidamente el dedo o autenticación incluso los datos que está protegidos por confianza chips del módulo de plataforma (TPM) en los equipos de usuarios. Aunque la autenticación multifactor no impide los ataques de robo de credenciales si un equipo ya está en peligro, proporcionando a los usuarios a los controles de autenticación fácil de usar, puede asignar contraseñas más sólidas para las cuentas de usuario para quien tradicional usuario los controles de nombre y la contraseña son difíciles de manejar.  
