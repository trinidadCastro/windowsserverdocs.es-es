---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: "Introducción"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dc89afc47eb78a388238e8edf5059b0bec3006ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="introduction"></a>Introducción

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Han existido ataques contra informática infraestructuras, si simples o complejas, como equipos tienen. Sin embargo, dentro de la última década, los números crecientes de organizaciones de todos los tamaños, en todas las partes del mundo se han atacado y puesto en peligro de maneras que han cambiado considerablemente el panorama de amenazas. No olvides warfare y ciberdelincuencia han aumentado a velocidades de registro. "Hacktivism", en el que se motivan ataques por activist posiciones, ha sido solicitados según lo previsto la motivación para un número de las infracciones de exponer la información de las organizaciones secreta, para crear negaciones de servicio, o incluso destruir infraestructura. Los ataques contra instituciones públicas y privadas con el objetivo de exfiltrating derechos de propiedad intelectual (IP) de las organizaciones han pasado a ser ubicuas.  
  
Ninguna organización con una infraestructura de TI de información es inmune de los ataques, pero si se implementen los controles, los procesos y las directivas apropiadas para proteger claves segmentos de infraestructura de la organización, escalación de sufrir ataques de penetración en peligro todo es que pueden evitar. Dado que el número y la escala de los ataques originados por fuera de una organización ha eclipsed amenaza de insider en los últimos años, a menudo tratados en este documento atacantes externos en lugar de uso incorrecto del entorno de los usuarios autorizados. No obstante, los principios y las recomendaciones proporcionadas en este documento están pensadas para ayudar a proteger su entorno contra los atacantes externos y los usuarios de Insider malintencionados o malintencionados.  
  
La información y las recomendaciones proporcionadas en este documento son procedentes de varios orígenes y deriva prácticas diseñadas para proteger instalaciones de Active Directory en peligro. Aunque no es posible evitar ataques, es posible para reducir la superficie de ataque de Active Directory y para implementar controles que hacen compromiso del directorio mucho más difícil para los atacantes. Este documento presenta a los tipos de las vulnerabilidades que tenemos más comunes observadas en entornos en riesgo y las recomendaciones más comunes que hemos realizado en los clientes para mejorar la seguridad de sus instalaciones de Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenciones de nomenclatura de grupo y la cuenta  
La siguiente tabla proporciona a una guía para las convenciones de nomenclatura que se usan en este documento para los grupos y cuentas que se hace referencia en todo el documento. Incluye en la tabla es la ubicación de cada grupo de cuentas, su nombre y cómo se hace referencia a estos grupos de cuentas en este documento.  
  


|**Ubicación de grupo o cuenta**|**Nombre de cuenta o grupo**|**¿Cómo se hace referencia en este documento**|
| --- | --- | --- |   
|Active Directory - cada dominio|Administrador|Cuenta predefinida de administrador|  
|Active Directory - cada dominio|Administradores|Grupo de administradores (BA) integrado|  
|Active Directory - cada dominio|Administradores de dominio|Grupo de administradores (DA)|  
|Active Directory - dominio raíz del bosque|Administradores de empresa|Grupo de administradores de empresa (EA)|  
|Base de datos de administrador (SAM) de cuentas de equipo local seguridad en equipos que ejecutan Windows Server y estaciones de trabajo que no son los controladores de dominio|Administrador|Cuenta de administrador local|  
|Base de datos de administrador (SAM) de cuentas de equipo local seguridad en equipos que ejecutan Windows Server y estaciones de trabajo que no son los controladores de dominio|Administradores|Grupo de administradores local|  
  
## <a name="about-this-document"></a>Acerca de este documento  
La seguridad de la información de Microsoft y la organización de administración de riesgo (ISRM), que forma parte de Microsoft información tecnología (MSIT), funciona con unidades de negocio internas, los clientes externos y sistemas del mismo nivel del sector para recopilar, distribuir y definen directivas, los procedimientos recomendados y los controles. Esta información puede usarse por Microsoft y nuestros clientes para aumentar la seguridad y reducir la superficie de ataque de sus infraestructuras de TI. Las recomendaciones proporcionadas en este documento se basan en un número de fuentes de información y procedimientos recomendados que se usa dentro de MSIT y ISRM. Las siguientes secciones presentan más información sobre los orígenes de este documento.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT y ISRM  
Se han desarrollado un número de procedimientos recomendados y los controles dentro de MSIT y ISRM para proteger los dominios y bosques de Microsoft AD DS. Cuando estos controles están ampliamente aplicables, se han integrado en este documento. SAFE-T (aceleradores de soluciones para las nuevas tecnologías) es un equipo en ISRM cuya compromiso es para identificar las tecnologías emergentes y definir los requisitos de seguridad y los controles para acelerar su adopción.  
  
### <a name="active-directory-security-assessments"></a>Evaluaciones de seguridad de Active Directory  
Dentro de Microsoft ISRM, la evaluación, consultoría y equipo de ingeniería (ACE) funciona con unidades de negocio internas de Microsoft y los clientes externos para evaluar la aplicación y la infraestructura de seguridad y para proporcionar instrucciones táctica y estratégica para aumentar el nivel de seguridad de la organización. Una oferta de servicio ACE es la evaluación de seguridad para Active Directory (ADSA), que es una evaluación integral del entorno de AD DS de la organización que evalúa personas, procesos y tecnología y produce recomendaciones específicas para el cliente. Los clientes se proporcionan recomendaciones basadas en características únicas de la organización, procedimientos y gustos de riesgo. Se han realizado ADSAs para instalaciones de Active Directory de Microsoft, además de los de nuestros clientes. Con el tiempo, se han encontrado una serie de recomendaciones es aplicable a través de los clientes de distintos tamaños y sectores.  
  
### <a name="content-origin-and-organization"></a>Organización y el origen de contenido  
Gran parte del contenido de este documento se deriva de la ADSA y otras evaluaciones equipo ACE que se lleva a cabo para clientes en riesgo y que no han sufrido compromiso significativo. Aunque los datos del cliente individuales no se usan para crear este documento, hemos recopilado las vulnerabilidades normalmente explotadas hemos identificado en nuestras evaluaciones y las recomendaciones hemos realizado en los clientes para mejorar la seguridad de sus instalaciones de AD DS. No todas las vulnerabilidades son aplicables a todos los entornos, ni tampoco son todas las recomendaciones factible para implementar en cada organización.  
  
Este documento se organiza de la siguiente manera:  
  
## <a name="executive-summary"></a>Resumen  
El ejecutivo resumen, que se pueden leer como un documento independiente o en combinación con el documento completo, proporciona un resumen de alto nivel de este documento. Incluido en el resumen ejecutivo son los vectores de ataque más comunes que hemos observado usada para comprometer los entornos de cliente resumen recomendaciones para proteger instalaciones de Active Directory y objetivos básicos para los clientes que va a implementar nuevas bosques de AD DS ahora o en el futuro.  
  
### <a name="introduction"></a>Introducción  
Esta es la sección que estás leyendo ahora.  
  
### <a name="avenues-to-compromise"></a>Vías peligro  
Esta sección proporciona información sobre algunas de las más comúnmente aprovecha las vulnerabilidades que hemos encontrado a atacantes pueden usar para poner en peligro infraestructuras de los clientes. Esta sección comienza con las categorías generales de las vulnerabilidades y cómo se aprovechar para inicialmente atraviese infraestructuras de los clientes, propagación comprometer en sistemas adicionales y finalmente centrarte en AD DS y controladores de dominio para obtener el control total de los bosques de las organizaciones.  
  
En esta sección no proporciona recomendaciones detalladas sobre direccionamiento cada tipo de vulnerabilidad, especialmente en las áreas en las que las vulnerabilidades no se usan para dirigirse directamente a Active Directory. Sin embargo, para cada tipo de vulnerabilidad, proporcionamos vínculos a información adicional que puede utilizar para desarrollar contramedidas y reducir la superficie de ataque de la organización.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Reducir la superficie de ataque de Active Directory  
Esta sección comienza proporcionando información básica acerca de las cuentas con privilegios y los grupos de Active Directory para proporcionar la información que ayuda a aclarar los motivos de las siguientes recomendaciones para protección y administración de cuentas y grupos con privilegios. A continuación, veremos enfoques para reducir la necesidad de usar cuentas con privilegios elevados para la administración diaria, que no requiere el nivel de privilegio que se concede a grupos como los grupos Administradores de empresa (EA), los administradores de dominio (DA) y los administradores integrados (BA) en Active Directory. A continuación, proporcionamos instrucciones para proteger las cuentas y grupos con privilegios y para implementar sistemas y prácticas administrativas seguras.  
  
Aunque esta sección proporciona información detallada sobre estas opciones de configuración, también hemos incluido apéndices para cada recomendación que proporcionan instrucciones paso a paso de configuración que pueden ser usadas "como está" o pueden modificarse para las necesidades de la organización. En esta sección se termina proporcionando información para implementar y administrar controladores de dominio, que deben ser entre los sistemas más estrictas protegidos en la infraestructura de forma segura.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory para signos de compromiso  
Si ha implementado supervisión (SIEM) en el entorno de eventos e información de seguridad eficaz o se mediante otros mecanismos para supervisar la seguridad de la infraestructura, esta sección proporciona información que puede usarse para identificar los eventos en los sistemas de Windows que pueden indicar que se ataca una organización. Trataremos las directivas de auditoría tradicional y avanzadas, incluidos eficaz configuración de subcategorías de auditoría en los sistemas operativos Windows 7 y Windows Vista. Esta sección incluye listas completas de objetos y sistemas de auditoría de un apéndice asociado listas y eventos para el que deben controlar si el objetivo es detectar los intentos de comprometer.  
  
### <a name="planning-for-compromise"></a>Planeación de comprometer  
Esta sección comienza pasando"Atrás" de los detalles técnicos centrarse en los principios y procesos que se puede implementar para identificar los usuarios, aplicaciones y sistemas que sean más importantes, no solo a la infraestructura de TI, pero para el negocio. Después de identificar novedades más importante para las operaciones de la organización y la estabilidad, puedes centrarte en separar y proteger estos activos, independientemente de si están sistemas, contactos o propiedad intelectual. En algunos casos, se pueden realizar separar y proteger activos en el entorno de AD DS existente, mientras que en otros casos, debería considerar implementar "celdas" pequeñas, independientes que te permiten establecer un límite seguro alrededor de los activos críticos y supervisar esos activos más estrictas que componentes menos críticos. Se explica el concepto de "destrucción creativa", que es un mecanismo mediante el cual se pueden eliminar aplicaciones heredadas y sistemas mediante la creación de nuevas soluciones y la sección finaliza con las recomendaciones que pueden ayudar a mantener un entorno más seguro mediante la combinación de negocio e información de TI para construir una imagen detallada de lo que es un estado de funcionamiento normal. Si sabes lo que es normal que una organización, anomalías que podrían indicar ataques y peligros pueden identificarse más fácilmente.  
  
### <a name="summary-of-best-practice-recommendations"></a>Resumen de los procedimientos recomendados  
En esta sección se proporciona una tabla que se resume las recomendaciones incluidas en este documento y se ordena por prioridad relativa, además de proporcionar vínculos a donde encontrará más información sobre cada recomendación en el documento y sus apéndices.  
  
### <a name="appendices"></a>Apéndices  
Apéndices se incluyen en este documento para ampliar la información contenida en el cuerpo del documento. La lista de apéndices y una breve descripción de cada uno se incluye en la siguiente tabla.  
  
 
|**Apéndice**|**Descripción**|
| --- | --- | 
|[Apéndice B: cuentas con privilegios y grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Proporciona información general que te ayuda a identificar los usuarios y grupos, que debes centrarte en proteger porque se puedan aprovechar los atacantes comprometer la seguridad y destruir incluso la instalación de Active Directory.|  
|[Apéndice C: cuentas protegidas y grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contiene información sobre los grupos protegidos en Active Directory. También contiene información de personalización limitada (eliminación) de los grupos que se consideran grupos protegidos y que se ven afectadas por AdminSDHolder y SDProp.|  
|[Apéndice D: seguridad de las cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Se incluyen directrices para ayudar a proteger la cuenta Administrador en cada dominio del bosque.|  
|[Apéndice E: proteger los grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Se incluyen directrices para ayudar a proteger el grupo de administradores de empresa en el bosque.|  
|[Apéndice F: proteger grupos de administradores de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Se incluyen directrices para ayudar a proteger el grupo de administradores de dominio en cada dominio del bosque.|  
|[Apéndice G: proteger el grupo de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Se incluyen directrices para ayudar a proteger el grupo de administradores integrados en cada dominio en el bosque.|  
|[Apéndice H: proteger cuentas de administrador Local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contiene directrices para ayudar a proteger las cuentas de administrador locales y grupo de administradores en los servidores unidos a un dominio y estaciones de trabajo.|  
|[Apéndice I: crear administración de cuentas para cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Proporciona información para crear cuentas que tienen privilegios limitados y pueden controlarse estrictas, pero pueden usarse para rellenar grupos con privilegios en Active Directory cuando se requiere elevación temporal.|  
|[Apéndice L: eventos al Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Enumera los eventos para el que debe supervisar en su entorno.|  
|[Apéndice M: vínculos del documento y lectura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contiene una lista de lectura recomendado. También contiene una lista de vínculos a documentos externos y sus direcciones URL para que los lectores de duro copias de este documento pueden acceder a esta información.|  
  


