---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introducción
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e2717af6183944b26a71e55b36f31cef51cf2e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831876"
---
# <a name="introduction"></a>Introducción

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los ataques contra las infraestructuras informáticas, ya sean simples o complejas, han existido siempre que tengan equipos. No obstante, durante la última década, una cantidad creciente de organizaciones de todos los tamaños y de todas las partes del mundo han sido atacadas y puestas en peligro de maneras que han cambiado significativamente el panorama de las amenazas. La guerra y los delitos informáticos han aumentado a ritmo récord. "Hacktivismo", en el que los ataques están motivados por posturas posiciones, se ha declarado como la motivación de un número de infracciones destinadas a información secreta de la organización, para crear denegaciones de servicio, o incluso para destruir infraestructuras. Los ataques contra las instituciones públicas y privadas con el fin de extraer los derechos de propiedad intelectual (IP) de las organizaciones han extendido.  
  
Ninguna organización con una infraestructura tecnológica (TI) es inmune a los ataques, pero si se implementan los controles, los procesos y las directivas apropiadas para proteger segmentos clave de la infraestructura informática de la organización, aumento de los ataques desde es posible evitable penetración en completarse en peligro. Dado que el número y la escala de los ataques procedentes del exterior de una organización ha eclipsada amenaza interna en los últimos años, este documento describen a menudo los atacantes externos en lugar de un uso incorrecto del entorno los usuarios autorizados. Sin embargo, los principios y las recomendaciones proporcionadas en este documento están diseñadas para ayudar a proteger su entorno contra los intrusos externos y maliciosos o equivocados.  
  
La información y las recomendaciones proporcionadas en este documento son procedentes de varios orígenes y derivan de prácticas diseñadas para proteger las instalaciones de Active Directory en peligro. Aunque no es posible evitar ataques, es posible para reducir la superficie de ataque de Active Directory e implementar controles que hacen que poner en peligro el directorio mucho más difícil para los atacantes. Este documento presenta a los tipos más comunes de vulnerabilidades, tenemos observados en entornos en peligro y las recomendaciones más comunes que hemos realizado a los clientes para mejorar la seguridad de sus instalaciones de Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenciones de denominación de grupo y cuenta  
En la tabla siguiente proporciona a una guía para las convenciones de nomenclatura usados en este documento para los grupos y cuentas que se hace referencia en todo el documento. Incluido en la tabla es la ubicación de cada grupo de cuentas, su nombre y cómo se hace referencia a estos grupos de cuentas en este documento.  
  


|**Ubicación de cuenta o grupo**|**Nombre de cuenta o grupo**|**¿Cómo se hace referencia en este documento**|
| --- | --- | --- |   
|Active Directory: cada dominio|Administrador|Cuenta de administrador integrada|  
|Active Directory: cada dominio|Administradores|Grupo de administradores (BA) integrado|  
|Active Directory: cada dominio|Admins. del dominio|Grupo de administradores (DA) de dominio|  
|Active Directory: dominio raíz del bosque|Administradores de empresas|Grupo de administradores de Enterprise (EA)|  
|Base de datos de administrador (de seguridad SAM) de cuentas de equipo local security en los equipos que ejecutan Windows Server y las estaciones de trabajo que no son controladores de dominio|Administrador|Cuenta de administrador local|  
|Base de datos de administrador (de seguridad SAM) de cuentas de equipo local security en los equipos que ejecutan Windows Server y las estaciones de trabajo que no son controladores de dominio|Administradores|Grupo de administradores locales|  
  
## <a name="about-this-document"></a>Acerca de este documento  
La organización de Microsoft Information Security and Risk Management (ISRM), que forma parte de la tecnología de información de Microsoft (MSIT), funciona con las unidades de negocio internas, los clientes externos y compañeros del sector para recopilar, distribuir y definir directivas, procedimientos recomendados y los controles. Esta información puede utilizarse por Microsoft y nuestros clientes para aumentar la seguridad y reducir la superficie de ataque de sus infraestructuras de TI. Las recomendaciones proporcionadas en este documento se basan en un número de fuentes de información y prácticas usadas dentro de MSIT y ISRM. Las secciones siguientes presentan más información acerca de los orígenes de este documento.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT y ISRM  
Se han desarrollado una serie de procedimientos recomendados y los controles dentro de MSIT y ISRM para proteger los dominios y bosques de AD DS de Microsoft. Cuando estos controles son ampliamente aplicables, se han integrado en este documento. SAFE-T (aceleradores de soluciones para las tecnologías emergentes) es un equipo dentro de ISRM cuyo contrato es identificar las tecnologías emergentes y para definir los requisitos de seguridad y controles para acelerar su adopción.  
  
### <a name="active-directory-security-assessments"></a>Evaluaciones de seguridad de Active Directory  
Dentro de Microsoft ISRM, la evaluación, consultoría y equipo Engineering (ACE) funciona con unidades de negocio internas de Microsoft y los clientes externos para evaluar la seguridad de la aplicación y la infraestructura y proporcionar una orientación táctica y estratégica para aumentar la posición de seguridad de la organización. Una oferta de servicio ACE es la evaluación de seguridad para Active Directory (ADSA), que es una evaluación integral del entorno de AD DS de la organización que evalúa las personas, procesos y tecnología y produce recomendaciones específicas para el cliente. Los clientes se proporcionan con las recomendaciones que se basan en características únicas de la organización, prácticas y apetito de riesgo. Se han realizado ADSAs para instalaciones de Active Directory en Microsoft, además de los que nuestros clientes. Con el tiempo, se han encontrado una serie de recomendaciones para aplicarse a los clientes de diferentes tamaños y sectores.  
  
### <a name="content-origin-and-organization"></a>Organización y origen del contenido  
Gran parte del contenido de este documento se deriva el ADSA y otras evaluaciones equipo ACE llevado a cabo por los clientes en peligro y los clientes que no han experimentado un riesgo significativo. Aunque los datos de clientes individuales no se usan para crear este documento, hemos recopilado las vulnerabilidades aprovechadas con más frecuencia que hemos identificado en nuestras evaluaciones y las recomendaciones hemos realizado a los clientes para mejorar la seguridad de su AD DS instalaciones. No todas las vulnerabilidades son aplicables a todos los entornos, ni todas las recomendaciones son viables para implementares en todas las organizaciones.  
  
Este documento está organizado como sigue:  
  
## <a name="executive-summary"></a>Resumen ejecutivo  
El ejecutivo resumen, que puede leerse como un documento independiente o en combinación con el documento completo, se proporciona un resumen de alto nivel de este documento. Los vectores de ataque más comunes que hemos observado usada para comprometer los entornos del cliente, resumen recomendaciones para proteger las instalaciones de Active Directory y los objetivos básicos para los clientes que planean implementar nuevo AD DS se encuentran incluidos en el resumen ejecutivo bosques ahora o en el futuro.  
  
### <a name="introduction"></a>Introducción  
Esta es la sección que está leyendo ahora.  
  
### <a name="avenues-to-compromise"></a>Vías de compromiso  
Esta sección proporciona información acerca de algunas de las más habitualmente aprovecha las vulnerabilidades que hemos encontrado que se usará por los atacantes para poner en peligro las infraestructuras de los clientes. En esta sección comienza con categorías generales de las vulnerabilidades y cómo se aprovecha para inicialmente penetrar en infraestructuras de los clientes, propagar compromiso entre sistemas adicionales y detectar finalmente AD DS y controladores de dominio para obtener una completa control de los bosques de organizaciones.  
  
En esta sección no proporciona recomendaciones detalladas acerca de cómo tratar cada tipo de vulnerabilidad, especialmente en las áreas en que las vulnerabilidades no se usan para tener como destino directamente de Active Directory. Sin embargo, para cada tipo de vulnerabilidad, se proporcionan vínculos a información adicional que puede usar para desarrollar contramedidas y reducir la superficie expuesta a ataques de su organización.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Reducción de la superficie de ataque de Active Directory  
En esta sección comienza con la que se proporciona información general acerca de las cuentas con privilegios y grupos en Active Directory para proporcionar la información que ayuda a aclarar las razones para las siguientes recomendaciones para proteger y administrar grupos con privilegios y cuentas. Luego, analizaremos los enfoques para reducir la necesidad de usar cuentas con privilegios elevados para la administración diaria, que no requiere que el nivel de privilegio que se concede a los grupos como administradores de Enterprise (EA), los administradores de dominio (DA) e integrada Grupos de administradores (BA) en Active Directory. A continuación, se proporcionan instrucciones para proteger las cuentas y grupos con privilegios y de implementación de sistemas y prácticas administrativas seguras.  
  
Aunque en esta sección se proporciona información detallada sobre estas opciones de configuración, también hemos incluido los apéndices de cada recomendación que se proporcionan instrucciones paso a paso de configuración que pueden ser utilizado "tal cual" o se pueden modificar para el necesidades de la organización. En esta sección se finaliza al proporcionar información para implementar y administrar controladores de dominio, que deben ser entre los sistemas más rigurosa protegidos en la infraestructura de forma segura.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory en busca de indicios de riesgo  
Si ha implementado la supervisión (SIEM) en su entorno de eventos e información de seguridad sólida o utiliza otros mecanismos para supervisar la seguridad de la infraestructura, esta sección proporciona información que puede usarse para identificar los eventos en Windows sistemas que puedan indicar que una organización está siendo atacada. Se describen las directivas de auditoría tradicional y avanzada, incluida la configuración eficaz de las subcategorías de auditoría en los sistemas operativos Windows 7 y Windows Vista. Esta sección incluyen listas completas de objetos y los sistemas de auditoría y un apéndice asociado enumera los eventos para el que debe supervisar si el objetivo es detectar los intentos de poner en peligro.  
  
### <a name="planning-for-compromise"></a>Planeación de compromiso  
En esta sección comienza pasando"Atrás" de los detalles técnicos para centrarse en los principios y procesos que se pueden implementar para identificar los usuarios, aplicaciones y sistemas que son más importantes no solo para la infraestructura de TI, pero para la empresa. Después de identificar lo que es más fundamental a la estabilidad y operaciones de su organización, puede centrarse en separar y proteger estos recursos, ya sean de propiedad intelectual, las personas o sistemas. En algunos casos, se pueden realizar separar y proteger los activos en el entorno existente de AD DS, mientras que en otros casos, debería considerar implementar "cells" pequeñas, independientes que permiten establecer un límite en torno a los recursos críticos seguro y supervisarlas los activos más rigurosamente que los componentes de menos críticos. Se describe un concepto denominado "destrucción creative", que es un mecanismo por el que se pueden eliminar los sistemas y aplicaciones heredadas mediante la creación de nuevas soluciones y la sección termina con las recomendaciones que pueden ayudar a mantener un entorno más seguro por combinación de negocio y la información de TI para construir una imagen detallada de lo que es un estado operativo normal. Sabiendo qué es normal para una organización, las anomalías que pueden indicar ataques y compromisos pueden identificarse más fácilmente.  
  
### <a name="summary-of-best-practice-recommendations"></a>Resumen de prácticas recomendadas  
Esta sección proporciona una tabla que resume las recomendaciones realizadas en este documento y los ordena por prioridad relativa, además de proporcionar vínculos a donde se puede encontrar más información acerca de cada recomendación en el documento y sus apéndices.  
  
### <a name="appendices"></a>Anexos  
Apéndices se incluyen en este documento para aumentar la información contenida en el cuerpo del documento. La lista de apéndices y una breve descripción de cada uno de ellos se incluye en la tabla siguiente.  
  
 
|**Apéndice**|**Descripción**|
| --- | --- | 
|[Apéndice B: Las cuentas con privilegios y grupos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Proporciona información general que le ayuda a identificar los usuarios y grupos que deben centrarse en proteger porque pueden aprovechar los atacantes para poner en peligro y destruir incluso su instalación de Active Directory.|  
|[Apéndice C: Cuentas protegidas y grupos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contiene información sobre grupos protegidos en Active Directory. También contiene información de personalización limitada (eliminación) de los grupos que se consideran grupos protegidos y se ven afectados por AdminSDHolder y SDProp.|  
|[Apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contiene directrices para ayudar a proteger la cuenta de administrador en cada dominio del bosque.|  
|[Apéndice E: Protección de grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contiene directrices para ayudar a proteger el grupo de administradores de empresas del bosque.|  
|[Apéndice F: Protección de grupos de administradores de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contiene directrices para ayudar a proteger el grupo Admins. del dominio en cada dominio del bosque.|  
|[Apéndice G: Protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contiene directrices para ayudar a proteger el grupo de administradores integrado en cada dominio del bosque.|  
|[Apéndice H: Protección de cuentas de administrador Local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contiene directrices para ayudar a proteger las cuentas de administrador locales y grupos de administradores de servidores unidos al dominio y estaciones de trabajo.|  
|[Apéndice I: Creación de la administración de cuentas para cuentas protegidas y grupos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Proporciona información para crear las cuentas con privilegios limitados y pueden controlarse rigurosamente, pero pueden usarse para rellenar los grupos con privilegios en Active Directory cuando se requiere la elevación temporal.|  
|[Apéndice L: Eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Las listas de eventos para el que debe supervisar en su entorno.|  
|[Apéndice M: Vínculos a documentos y lecturas recomendadas](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contiene una lista de lectura recomendada. También contiene una lista de vínculos a documentos externos y sus direcciones URL para que los lectores de las copias impresas de este documento pueden acceder a esta información.|  
  


