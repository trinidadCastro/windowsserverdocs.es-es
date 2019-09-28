---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introducción
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab21cca727342f6dc69ceecfb0c8991b30b0f227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389882"
---
# <a name="introduction"></a>Introducción

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los ataques contra las infraestructuras informáticas, ya sean simples o complejas, han existido siempre que los equipos tengan. No obstante, durante la última década, una cantidad creciente de organizaciones de todos los tamaños y de todas las partes del mundo han sido atacadas y puestas en peligro de maneras que han cambiado significativamente el panorama de las amenazas. La guerra y los delitos informáticos han aumentado a ritmo récord. "Hacktivismo", en el que los ataques están motivados por las posiciones de posturas activistas, se ha reclamado como la motivación de una serie de infracciones destinadas a exponer la información secreta de la organización, para crear denegaciones de servicio o incluso para destruir la infraestructura. Ataques contra las instituciones públicas y privadas con el objetivo de extraer la propiedad intelectual (IP) de las organizaciones se ha convertido en omnipresentes.  
  
Ninguna organización con una infraestructura de tecnología de la información (TI) es inmune a los ataques, pero si se implementan las directivas, los procesos y los controles adecuados para proteger los segmentos clave de la infraestructura informática de una organización, la escala de los ataques desde la penetración para completar el riesgo podría ser evitable. Dado que el número y la escala de los ataques que se originan fuera de una organización tiene una amenaza de Insider Insider en los últimos años, en este documento se describen a menudo atacantes externos en lugar de un uso incorrecto del entorno por parte de usuarios autorizados. No obstante, los principios y las recomendaciones proporcionadas en este documento están diseñados para ayudar a proteger su entorno contra los atacantes externos y los Insiders malintencionados o malintencionados.  
  
La información y las recomendaciones proporcionadas en este documento se extraen de una serie de orígenes y se derivan de prácticas diseñadas para proteger las instalaciones Active Directory contra el riesgo. Aunque no es posible evitar ataques, es posible reducir la superficie expuesta a ataques de Active Directory e implementar controles que hagan que el directorio sea mucho más difícil para los atacantes. En este documento se presentan los tipos más comunes de vulnerabilidades que hemos observado en entornos en peligro y las recomendaciones más comunes que hemos realizado a los clientes para mejorar la seguridad de sus instalaciones Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenciones de nomenclatura de grupos y cuentas  
En la tabla siguiente se proporciona una guía de las convenciones de nomenclatura utilizadas en este documento para los grupos y las cuentas a los que se hace referencia en el documento. En la tabla se incluye la ubicación de cada cuenta o grupo, su nombre y cómo se hace referencia a estas cuentas o grupos en este documento.  
  


|**Ubicación de la cuenta o grupo**|**Nombre de la cuenta o grupo**|**Cómo se hace referencia en este documento**|
| --- | --- | --- |   
|Active Directory: cada dominio|Administrador|Cuenta de administrador integrada|  
|Active Directory: cada dominio|Administradores|Grupo administradores integrados (BA)|  
|Active Directory: cada dominio|Admins. del dominio|Grupo Admins. del dominio (DA)|  
|Active Directory: dominio raíz del bosque|Administradores de empresas|Grupo administradores de empresas (EA)|  
|Base de datos del administrador de cuentas de seguridad (SAM) del equipo local en equipos que ejecutan Windows Server y estaciones de trabajo que no son controladores de dominio|Administrador|Cuenta de administrador local|  
|Base de datos del administrador de cuentas de seguridad (SAM) del equipo local en equipos que ejecutan Windows Server y estaciones de trabajo que no son controladores de dominio|Administradores|Grupo Administradores local|  
  
## <a name="about-this-document"></a>Acerca de este documento  
La organización de Microsoft Information Security and Risk Management (ISRM), que forma parte de la tecnología de la información de Microsoft (MSIT), trabaja con unidades de negocio internas, clientes externos y colegas del sector para recopilar, difundir y definir directivas. prácticas y controles. Microsoft y nuestros clientes pueden usar esta información para aumentar la seguridad y reducir la superficie expuesta a ataques de sus infraestructuras de ti. Las recomendaciones proporcionadas en este documento se basan en una serie de orígenes de información y prácticas que se usan en MSIT y ISRM. En las secciones siguientes se presenta más información sobre los orígenes de este documento.  
  
### <a name="microsoft-it-and-isrm"></a>TI de Microsoft y ISRM  
Se han desarrollado varias prácticas y controles dentro de MSIT y ISRM para proteger los bosques y dominios de Microsoft AD DS. Cuando estos controles son ampliamente aplicables, se han integrado en este documento. SAFE-T (aceleradores de soluciones para tecnologías emergentes) es un equipo dentro de ISRM cuyo contrato consiste en identificar las tecnologías emergentes y definir los requisitos de seguridad y los controles para acelerar su adopción.  
  
### <a name="active-directory-security-assessments"></a>Active Directory evaluaciones de seguridad  
En Microsoft ISRM, el equipo de evaluación, consultoría e ingeniería (ACE) trabaja con las unidades de negocio internas de Microsoft y con los clientes externos para evaluar la seguridad de la infraestructura y la aplicación, y proporcionar una guía táctica y estratégica para aumentar el postura de seguridad de la organización. Una oferta de servicio ACE es la Active Directory de evaluación de seguridad (ADSA), que es una evaluación holística del entorno de AD DS de una organización que evalúa personas, procesos y tecnología y genera recomendaciones específicas del cliente. A los clientes se les ofrece recomendaciones basadas en las características únicas, las prácticas y el apetito de riesgo de la organización. ADSAs se han realizado para instalaciones Active Directory en Microsoft, además de las de nuestros clientes. Con el tiempo, se ha encontrado una serie de recomendaciones que se pueden aplicar a todos los clientes de diferentes tamaños y sectores.  
  
### <a name="content-origin-and-organization"></a>Organización y origen del contenido  
Gran parte del contenido de este documento se deriva de ADSA y otras evaluaciones del equipo ACE realizadas para clientes comprometidos y clientes que no han experimentado un riesgo significativo. Aunque los datos de clientes individuales no se usaron para crear este documento, hemos recopilado las vulnerabilidades aprovechadas con más frecuencia que hemos identificado en nuestras evaluaciones y las recomendaciones que hemos realizado a los clientes para mejorar la seguridad de sus AD DS atendida. No todas las vulnerabilidades son aplicables a todos los entornos, ni todas las recomendaciones son viables para implementares en todas las organizaciones.  
  
Este documento se organiza de la siguiente manera:  
  
## <a name="executive-summary"></a>Resumen ejecutivo  
El Resumen Ejecutivo, que se puede leer como un documento independiente o en combinación con el documento completo, proporciona un resumen de alto nivel de este documento. En el Resumen Ejecutivo se incluyen los vectores de ataque más comunes que se han observado para poner en peligro los entornos de cliente, recomendaciones de resumen para proteger Active Directory instalaciones y objetivos básicos para los clientes que planean implementar nuevas AD DS bosques ahora o en el futuro.  
  
### <a name="introduction"></a>Introducción  
Esta es la sección que está leyendo ahora.  
  
### <a name="avenues-to-compromise"></a>Vías de compromiso  
En esta sección se proporciona información acerca de algunas de las vulnerabilidades aprovechadas más comúnmente que han encontrado los atacantes para poner en peligro las infraestructuras de los clientes. Esta sección comienza con categorías generales de vulnerabilidades y cómo se aprovechan para penetrar inicialmente las infraestructuras de los clientes, propagar el compromiso entre sistemas adicionales y, finalmente, dirigirse a AD DS y controladores de dominio para obtener el completo control de los bosques de la organización.  
  
En esta sección no se proporcionan recomendaciones detalladas sobre cómo abordar cada tipo de vulnerabilidad, especialmente en las áreas en las que no se usan las vulnerabilidades para tener como destino directo Active Directory. Sin embargo, para cada tipo de vulnerabilidad, se proporcionan vínculos a información adicional que puede usar para desarrollar contramedidas y reducir la superficie expuesta a ataques de su organización.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Reducción de la superficie de ataque de Active Directory  
En esta sección, se proporciona información general acerca de los grupos y las cuentas con privilegios en Active Directory para proporcionar la información que le ayude a aclarar las razones de las recomendaciones posteriores para proteger y administrar grupos con privilegios y contabilidad. A continuación, se analizan los enfoques para reducir la necesidad de usar cuentas con privilegios elevados para la administración diaria, que no requiere el nivel de privilegio que se concede a grupos como administradores de empresas (EA), administradores de dominio (DA) e integrados. Administradores (BA) en Active Directory. A continuación, se proporcionan instrucciones para proteger los grupos y las cuentas con privilegios, así como para implementar sistemas y procedimientos administrativos seguros.  
  
Aunque en esta sección se proporciona información detallada acerca de estas opciones de configuración, también hemos incluido los apéndices para cada recomendación que proporcionan instrucciones de configuración paso a paso que se pueden usar "tal cual" o que se pueden modificar para el necesidades de la organización. En esta sección, se proporciona información para implementar y administrar de forma segura los controladores de dominio, que deben estar entre los sistemas protegidos más rigurosamente en la infraestructura.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory en busca de indicios de riesgo  
Si ha implementado la sólida información de seguridad y la supervisión de eventos (SIEM) en su entorno o usa otros mecanismos para supervisar la seguridad de la infraestructura, en esta sección se proporciona información que se puede usar para identificar eventos en Windows. sistemas que pueden indicar que una organización está sufriendo un ataque. Analizamos las directivas de auditoría tradicionales y avanzadas, incluida la configuración efectiva de las subcategorías de auditoría en los sistemas operativos Windows 7 y Windows Vista. En esta sección se incluyen listas completas de objetos y sistemas que se van a auditar, y un apéndice asociado enumera eventos para los que se debe supervisar si el objetivo es detectar intentos de ataque.  
  
### <a name="planning-for-compromise"></a>Planeación de compromiso  
En esta sección se comienza por "depuración" de detalles técnicos para centrarse en los principios y procesos que se pueden implementar para identificar los usuarios, las aplicaciones y los sistemas que son más críticos no solo para la infraestructura de ti, sino también para la empresa. Después de identificar lo que es más importante para la estabilidad y las operaciones de su organización, puede centrarse en separar y proteger estos recursos, independientemente de que sean propiedad intelectual, personas o sistemas. En algunos casos, se pueden separar y proteger los recursos en el entorno de AD DS existente, mientras que en otros casos, se debe considerar la posibilidad de implementar pequeñas y separadas "celdas" que permiten establecer un límite seguro en torno a los recursos críticos y supervisarlos activos más rigurosamente que los componentes menos críticos. Un concepto denominado "destrucción creativa", que es un mecanismo por el que se pueden eliminar aplicaciones y sistemas heredados mediante la creación de nuevas soluciones, y la sección finaliza con recomendaciones que pueden ayudar a mantener un entorno más seguro. combinar información empresarial y de TI para crear una imagen detallada de lo que es un estado operativo normal. Al saber qué es normal para una organización, las anomalías que pueden indicar ataques y peligros se pueden identificar más fácilmente.  
  
### <a name="summary-of-best-practice-recommendations"></a>Resumen de las recomendaciones de prácticas recomendadas  
En esta sección se proporciona una tabla en la que se resumen las recomendaciones realizadas en este documento y se ordenan por prioridad relativa, además de proporcionar vínculos a los que se puede encontrar más información sobre cada recomendación en el documento y sus apéndices.  
  
### <a name="appendices"></a>Anexos  
En este documento se incluyen los apéndices para aumentar la información contenida en el cuerpo del documento. En la tabla siguiente se incluye la lista de apéndices y una breve descripción de cada uno.  
  
 
|**Apéndice**|**Descripción**|
| --- | --- | 
|[Apéndice B: Cuentas con privilegios y grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Proporciona información general que le ayudará a identificar los usuarios y grupos que debe centrarse en la protección, ya que pueden ser aprovechados por los atacantes para poner en peligro e incluso destruir la instalación de Active Directory.|  
|[Apéndice C: Cuentas protegidas y grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contiene información acerca de los grupos protegidos en Active Directory. También contiene información para la personalización limitada (desinstalación) de los grupos que se consideran grupos protegidos y que se ven afectados por AdminSDHolder y SDProp.|  
|[Apéndice D: Protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contiene instrucciones para ayudar a proteger la cuenta de administrador en cada dominio del bosque.|  
|[Apéndice E: Protección de grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contiene instrucciones para ayudar a proteger el grupo administradores de organización en el bosque.|  
|[Apéndice F: Protección de grupos de administradores de dominio en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contiene instrucciones para ayudar a proteger el grupo Admins. del dominio en cada dominio del bosque.|  
|[Apéndice G: Protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contiene instrucciones para ayudar a proteger el grupo de administradores integrado en cada dominio del bosque.|  
|[Apéndice H: Protección de cuentas de administrador local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contiene instrucciones para ayudar a proteger las cuentas de administrador local y los grupos de administradores en estaciones de trabajo y servidores Unidos a un dominio.|  
|[Apéndice I: Creación de cuentas de administración para cuentas protegidas y grupos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Proporciona información para crear cuentas con privilegios limitados y que se pueden controlar de forma rigurosa, pero se pueden usar para rellenar grupos con privilegios en Active Directory cuando se requiere una elevación temporal.|  
|[Apéndice L: Eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Muestra los eventos que debe supervisar en su entorno.|  
|[Apéndice M: Vínculos a documentos y lecturas recomendadas](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contiene una lista de lecturas recomendadas. También contiene una lista de vínculos a documentos externos y sus direcciones URL para que los lectores de copias impresas de este documento puedan tener acceso a esta información.|  
  


