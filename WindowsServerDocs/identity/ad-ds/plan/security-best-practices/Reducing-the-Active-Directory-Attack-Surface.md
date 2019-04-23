---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Reducción de la superficie de ataque de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d692641d316b5fe7206cc3f413bdcfc9b74675b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874156"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Reducción de la superficie de ataque de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En esta sección se centra en los controles técnicos para implementar para reducir la superficie de ataque de la instalación de Active Directory. La sección contiene la siguiente información:  
  
-   [Implementación de modelos administrativos de menor privilegio](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) se centra en identificar el riesgo de que el uso de privilegios elevados de cuentas para la administración diaria se presenta, además de proporcionar recomendaciones para implementar para reducir el riesgo cuentas presente ese con privilegios.  
  
-   [Implementación de Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) describe los principios de enfoques de implementación de sistemas administrativos dedicados y seguros, además de algunos ejemplos de una implementación de host administrativo seguro.  
  
-   [Protección de controladores de dominio frente a ataque](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) analiza las directivas y configuraciones que, aunque son similares a las recomendaciones para la implementación de hosts administrativos seguros, contienen algunas recomendaciones específicas del controlador de dominio para ayudar a Asegúrese de que los controladores de dominio y los sistemas se usan para administrarlos están bien protegidos.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Las cuentas con privilegios y grupos en Active Directory  
Esta sección proporciona información general acerca de las cuentas con privilegios y grupos en Active Directory pretenden explicar las similitudes y diferencias entre las cuentas con privilegios y grupos en Active Directory. Al comprender estas diferencias, tanto si implementa las recomendaciones de [implementar modelos administrativos de menor privilegio](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuales o elija Personalizar para su organización, tiene las herramientas que necesita para proteger cada grupo y la cuenta correctamente.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Integrado con privilegios a cuentas y grupos  
Active Directory facilita la delegación de administración y es compatible con el principio de privilegio mínimo en la asignación de derechos y permisos. "Normales" usuarios que tienen cuentas en un dominio son, de forma predeterminada, puede leer gran parte de lo que se almacena en el directorio, pero pueden cambiar solo un conjunto muy limitado de datos en el directorio. Pertenencia a grupos "privilegiados" que se integran en el directorio para que pueden realizar tareas específicas relacionadas con sus roles, pero no se pueden realizar las tareas que no son relevantes para sus tareas se pueden conceder a los usuarios que requieren privilegios adicionales. Las organizaciones también pueden crear grupos que se adaptan a las responsabilidades de trabajo específico y se conceden derechos granulares y los permisos que permiten el personal de TI realizar funciones administrativas diarias sin conceder derechos y permisos que superan lo que se requiere para esas funciones.  
  
Dentro de Active Directory, los tres grupos integrados son los grupos de privilegio más altos en el directorio: Administradores de organización, Admins. del dominio y administradores. La configuración predeterminada y las capacidades de cada uno de estos grupos se describen en las secciones siguientes:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Grupos de privilegios más altos en Active Directory  
  
##### <a name="enterprise-admins"></a>Administradores de empresas  
Los administradores de Enterprise (EA) es un grupo que solo existe en el dominio raíz del bosque y, de forma predeterminada, es un miembro del grupo Administradores en todos los dominios del bosque. La cuenta predefinida de administrador en el dominio raíz del bosque es el único miembro predeterminado del grupo de EA. EAs se conceden derechos y permisos que les permitan implementar cambios en todo el bosque (es decir, los cambios que afectan a todos los dominios del bosque), como agregar o quitar dominios, establecer confianzas de bosque o elevar los niveles funcionales del bosque. En un modelo de delegación correctamente diseñado e implementado, suscripción EA se requiere únicamente cuando construir primero el bosque o al realizar determinados cambios en todo el bosque, como establecer una relación de confianza de bosque de salida. La mayoría de los derechos y permisos concedidos al grupo de EA puede delegarse a grupos y usuarios, con menos privilegios.  
  
##### <a name="domain-admins"></a>Admins. del dominio  

Cada dominio en un bosque tiene su propio grupo de administradores de dominio (DA), que es un miembro del grupo de administradores del dominio y un miembro del grupo Administradores local en cada equipo que está unido al dominio. El único miembro predeterminado del grupo DA para un dominio es la cuenta predefinida de administrador para ese dominio. DAs son "todopoderosos" dentro de sus dominios, mientras que EAs tienen privilegios para todo el bosque. En un modelo de delegación correctamente diseñado e implementado, se debe necesario ser miembro de Admins. del dominio solo en los casos "de emergencia" (por ejemplo, en situaciones en que se necesita una cuenta con altos niveles de privilegios en todos los equipos del dominio). Aunque los mecanismos de delegación de Active Directory nativos permiten la delegación en la medida en que es posible usar cuentas DA solo en los casos de emergencias, construir un modelo de delegación eficaz puede llevar mucho tiempo y sacar provecho de muchas organizaciones herramientas de terceros para acelerar el proceso.  
  
##### <a name="administrators"></a>Administradores  
El tercer grupo es el grupo de administradores (BA) local de dominio integrado en la que se anidan DAs y EAs. Este grupo tiene muchos de los directos derechos y permisos en el directorio y en los controladores de dominio. Sin embargo, el grupo de administradores de un dominio no tiene privilegios en los servidores miembro o en estaciones de trabajo. Es a través de la pertenencia al grupo Administradores local de los equipos que se concede el privilegio local.  
  
> [!NOTE]  
> Aunque estas son las configuraciones predeterminadas de estos grupos con privilegios, un miembro de cualquiera de los tres grupos puede manipular el directorio para ser miembro de cualquiera de los otros grupos. En algunos casos, es muy fácil de obtener la pertenencia de los otros grupos, mientras que en otros es más difícil, pero desde la perspectiva de privilegios posibles, los tres grupos deben considerarse equivalentes efectivo.  
  
##### <a name="schema-admins"></a>Administradores de esquema  

Un cuarto con privilegios, grupo de administradores de esquema (SA), sólo existe en el dominio raíz del bosque y tiene solo de ese dominio cuenta predefinida Administrador como un miembro predeterminado, que es similar al grupo Administradores de empresas. El grupo de administradores de esquema está pensado para que se rellena solo temporalmente y, en ocasiones (cuando se requiere la modificación del esquema de AD DS).  
  
Aunque el grupo de SA es el único grupo que puede modificar el esquema de Active Directory (que lo sea., estructuras de datos subyacentes del directorio, como los objetos y atributos), el ámbito de los derechos y permisos del grupo de SA está más limitado que los descritos anteriormente grupos. También es común encontrar que las organizaciones han desarrollado prácticas adecuadas para la administración de la pertenencia del grupo SA porque la pertenencia al grupo suele ser necesaria con poca frecuencia y solo durante breves períodos de tiempo. Esto es técnicamente cierto con el EA, DA y BA grupos en Active Directory, también, pero es mucho menos común encontrar que las organizaciones han implementado prácticas similares para estos grupos como para el grupo de SA.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Cuentas protegidas y grupos en Active Directory  
Dentro de Active Directory, un conjunto predeterminado de las cuentas con privilegios y grupos denominadas "protegidas" cuentas y grupos están protegidos de forma diferente a otros objetos en el directorio. Cualquier cuenta que tiene pertenencia directa o transitiva en cualquier grupo protegido (independientemente de si la pertenencia se deriva de los grupos de distribución o de seguridad) hereda esta seguridad restringido.  

  
Por ejemplo, si un usuario es miembro de un grupo de distribución es decir, a su vez, un miembro de un grupo en Active Directory, ese objeto de usuario protegido se marca como una cuenta protegida. Cuando una cuenta se marca como una cuenta protegida, el valor del atributo adminCount en el objeto se establece en 1.  
  
> [!NOTE]
> Aunque la pertenencia transitiva en un grupo protegido incluye distribución anidado y grupos de seguridad anidados, las cuentas que son miembros de grupos de distribución anidado no recibirá a SID del grupo protegido en sus tokens de acceso. Sin embargo, los grupos de distribución pueden convertirse en grupos de seguridad de Active Directory, que es el motivo por grupos de distribución se incluyen en la enumeración de miembros del grupo protegido. Un grupo de distribución anidado protegido alguna vez se debe convertir a un grupo de seguridad, las cuentas que son miembros del grupo de distribución anterior posteriormente, recibirá al elemento primario protejan a SID de grupo en sus tokens de acceso en el siguiente inicio de sesión.  
  
En la tabla siguiente se enumera las cuentas de protegidas de manera predeterminada y grupos en Active Directory de nivel de sistema operativo versión y service pack.  
  
**Cuentas protegidas y grupos en Active Directory por sistema operativo y versión del Service Pack (SP) predeterminados**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 <SP4**|**Windows 2000 SP4 -Windows Server 2003**|**Windows Server 2003 SP1+**|**Windows Server 2008 -Windows Server 2012**|  
|Administradores|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|  
||Administrador|Administrador|Administrador|  
||Administradores|Administradores|Administradores|  
|Admins. del dominio|Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|  
||Publicadores de certificados|||  
||Admins. del dominio|Admins. del dominio|Admins. del dominio|  
|Administradores de empresas|Controladores de dominio|Controladores de dominio|Controladores de dominio|  
||Administradores de empresas|Administradores de empresas|Administradores de empresas|  
||Krbtgt|Krbtgt|Krbtgt|  
||Opers. de impresión|Opers. de impresión|Opers. de impresión|  
||||Controladores de dominio de solo lectura|  
||Replicador|Replicador|Replicador|  
|Administradores de esquema|Administradores de esquema||Administradores de esquema|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder y SDProp  
En el contenedor del sistema de cada dominio de Active Directory, se crea automáticamente un objeto denominado AdminSDHolder. El propósito del objeto AdminSDHolder es garantizar que los permisos de cuentas protegidas y grupos se apliquen coherente, independientemente de dónde se encuentran las cuentas y grupos protegidos en el dominio.  

Cada 60 minutos (de forma predeterminada), un proceso conocido como propagador de descriptores de seguridad (SDProp) se ejecuta en el controlador de dominio que contiene el rol de emulador de PDC del dominio. SDProp compara los permisos en el objeto AdminSDHolder del dominio con los permisos en las cuentas protegidas y grupos del dominio. Si los permisos en cualquiera de los grupos y cuentas protegidas no coinciden con los permisos en el objeto AdminSDHolder, se restablecen los permisos en los grupos y cuentas protegidas para que coincidan con los del objeto AdminSDHolder del dominio.  
  
Herencia de permisos está deshabilitada en grupos protegidos y cuentas, lo que significa que incluso si las cuentas o grupos se mueven a ubicaciones diferentes en el directorio, no heredan permisos de sus objetos primarios nuevo. También se deshabilita la herencia en el objeto AdminSDHolder para que los cambios de permisos a los objetos primarios no cambian los permisos de AdminSDHolder.  
  
> [!NOTE]
> Cuando se quita una cuenta de un grupo protegido, ya no se considera una cuenta protegida, pero su permanece de atributo adminCount establecido en 1 si no se cambia manualmente. El resultado de esta configuración es que las ACL del objeto ya no se actualizan por SDProp, pero el objeto aún no hereda los permisos de su objeto primario. Por lo tanto, el objeto puede residir en una unidad organizativa (OU) a la que se han delegado permisos, pero el objeto protegido anteriormente no heredará estos permisos delegados. Puede encontrar un script para localizar y restablecer objetos protegidos anteriormente en el dominio en el [artículo de Microsoft Support 817433 los](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propiedad AdminSDHolder  
Mayoría de los objetos en Active Directory es propiedad del grupo del dominio BA. Sin embargo, el objeto AdminSDHolder es, de forma predeterminada, propiedad del grupo del dominio DA. (Esto es una circunstancia en la que no derivan DAs sus derechos y permisos mediante la pertenencia del grupo de administradores del dominio).  
  
En versiones de Windows anteriores a Windows Server 2008, los propietarios de un objeto pueden cambiar los permisos del objeto, incluyendo la concesión de permisos que no tenía originalmente a sí mismos. Por lo tanto, los permisos predeterminados en el objeto AdminSDHolder de un dominio evitar que los usuarios que son miembros de grupos BA o EA cambien los permisos para el objeto AdminSDHolder de un dominio. Sin embargo, pueden tomar posesión del objeto y concederse a sí mismos permisos adicionales, lo que significa que esta protección es rudimentaria y sólo protege el objeto con la modificación accidental de los usuarios miembros del grupo Administradores del dominio no los miembros del grupo DA en el dominio. Además, el BA y EA (si procede) grupos tienen permiso para cambiar los atributos del objeto AdminSDHolder en el dominio local (dominio raíz de EA).  
  
> [!NOTE]  
> Un atributo en el objeto AdminSDHolder, dSHeuristics, permite la personalización limitado (eliminación) de los grupos que se consideran grupos protegidos y se ven afectados por AdminSDHolder y SDProp. Esta personalización debe considerarse detenidamente si se implementa, aunque hay circunstancias válidos en el que la modificación de dSHeuristics en AdminSDHolder es útil. Puede encontrar más información acerca de la modificación del atributo dSHeuristics en un objeto AdminSDHolder en los artículos de Microsoft Support [817433 los](https://support.microsoft.com/?id=817433) y [973840](https://support.microsoft.com/kb/973840)y en [Apéndice C: Grupos de Active Directory y cuentas protegidas](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Aunque aquí se describen los grupos con más privilegios en Active Directory, hay una serie de otros grupos que se han concedido elevan niveles de privilegio. Para obtener más información acerca de todos los grupos predeterminados e integrados en Active Directory y los derechos de usuario asignados a cada uno, consulte [Apéndice B: Grupos de Active Directory y cuentas con privilegios](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


