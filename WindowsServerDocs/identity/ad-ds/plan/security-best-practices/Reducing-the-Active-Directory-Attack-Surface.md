---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Reducir la superficie de ataque de Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2de254076b10a1a75d658f006c2245d523de6b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="reducing-the-active-directory-attack-surface"></a>Reducir la superficie de ataque de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En esta sección se centra en los controles técnicos implementar para reducir la superficie de ataque de la instalación de Active Directory. La sección contiene la siguiente información:  
  
-   [Implementar modelos administrativos de privilegios mínimos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) se centra en la identificación el riesgo de que se presenta el uso de cuentas con privilegios elevados para la administración diaria, además de proporcionar recomendaciones para implementar para reducir el riesgo de que las cuentas que presente con privilegios.  
  
-   [Implementar seguro Hosts administrativas](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) describe principios para métodos de implementación de sistemas administrativos dedicados y seguros, además de algunos muestra una implementación de host administrativas seguro.  
  
-   [Proteger los controladores de dominio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) analiza las directivas y opciones de configuración que, aunque parezcan similares a las recomendaciones para la implementación de hosts administrativas seguros, contienen algunas recomendaciones específicos de mando de dominio para ayudar a garantizar que los controladores de dominio y los sistemas que se usa para administrarlas están bien protegidos.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Cuentas con privilegios y los grupos de Active Directory  
Esta sección proporciona información general sobre las cuentas con privilegios y grupos de Active Directory intenta explicar las similitudes y diferencias entre grupos y cuentas con privilegios en Active Directory. Si comprendes estas diferencias, tanto si implementa las recomendaciones indicadas en [implementar modelos administrativos de privilegios mínimos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuales o elige personalizarlas para la organización, dispones de las herramientas que necesitas para cada grupo de seguridad y cuenta correctamente.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Integrado privilegiadas cuentas y grupos  
Active Directory facilita la delegación de administración y admite el principio de privilegios mínimos en la asignación de derechos y permisos. Los usuarios "Normales" con cuentas de un dominio son, de manera predeterminada, puede leer gran parte de lo que se almacena en el directorio, pero pueden cambiar solo un conjunto muy limitado de datos en el directorio. Los usuarios que requieren privilegios adicionales pueden tener la pertenencia a grupos "privilegiadas" que están integrados en el directorio para que se pueden realizar tareas específicas relacionadas con sus roles, pero no puede realizar tareas que no son relevantes para sus funciones. Las organizaciones también pueden crear grupos que están adaptados a las funciones específicas y se otorgan derechos granulares y permisos que permiten el personal de TI realizar funciones administrativas diarias sin conceder derechos y permisos que superen lo que es necesario para esas funciones.  
  
En Active Directory, tres grupos integrados son los grupos de privilegios más altos en el directorio: los administradores de empresa, administradores de dominio y administradores. En las siguientes secciones se describen las capacidades de cada uno de estos grupos y la configuración predeterminada:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Grupos de privilegios más altos en Active Directory  
  
##### <a name="enterprise-admins"></a>Administradores de empresa  
Administradores de empresa (EA) es un grupo que existe solo en el dominio raíz y de manera predeterminada, es un miembro del grupo Administradores en todos los dominios del bosque. La cuenta predefinida de administrador en el dominio raíz es el único miembro predeterminado del grupo de EA. EAs se otorgan derechos y permisos que permiten llevar a cabo cambios en todo el bosque (es decir, cambios que afectan a todos los dominios del bosque), como agregar o quitar dominios, establecer relaciones de confianza de bosque o generar niveles funcionales de bosque. En un modelo de delegación correctamente diseñada e implementado, pertenencia EA es necesario solo cuando se construye primero el bosque o cuando se realicen cambios en todo el bosque, como establecer una relación de confianza de bosque de salida. La mayoría de los derechos y permisos otorgados al grupo EA puede delegarse a menor privilegios usuarios y grupos.  
  
##### <a name="domain-admins"></a>Administradores de dominio  

Cada dominio en un bosque tiene su propio grupo de administradores de dominio (DA), que es un miembro del grupo de administradores del dominio y un miembro del grupo de administradores local en cada equipo que está unido al dominio. El único miembro predeterminado del grupo DA para un dominio es la cuenta predefinida de administrador para ese dominio. DAs son "todopoderosos" dentro de sus dominios, mientras que EAs tienen privilegios de todo el bosque. En un modelo de delegación correctamente diseñada e implementado, pertenencia a administradores de dominio deben únicamente en escenarios "break vidrio" (por ejemplo, en situaciones en que se necesita una cuenta con privilegios elevados en todos los equipos del dominio). Aunque los mecanismos de delegación de Active Directory nativos permiten la delegación en la medida en que es posible usar cuentas de DA solo en situaciones de emergencia, construir un modelo de delegación eficaz puede tardar mucho tiempo y muchas organizaciones aprovechan las herramientas de terceros para acelerar el proceso.  
  
##### <a name="administrators"></a>Administradores  
El tercer grupo es el grupo de administradores (BA) local de dominio integrados que están anidados DAs y EAs. Este grupo tiene muchos de los derechos directos y permisos en el directorio y en los controladores de dominio. Sin embargo, el grupo Administradores de un dominio no tiene privilegios en los servidores miembro o en estaciones de trabajo. Es a través de la pertenencia a grupo Administradores local los equipos que se concede privilegios local.  
  
> [!NOTE]  
> Aunque se trata de las configuraciones predeterminadas de estos grupos con privilegios, un miembro de cualquiera de los tres grupos puede manipular el directorio para obtener la pertenencia a cualquiera de los otros grupos. En algunos casos, es muy fácil de obtener la pertenencia a los otros grupos, mientras que en otros es más difícil, pero desde la perspectiva de privilegios posibles, los tres grupos deben considerarse realmente el equivalente.  
  
##### <a name="schema-admins"></a>Administradores de esquema  

Un cuarto privilegiadas grupo, administradores de esquema (SA), existe solo en el dominio raíz del bosque y tiene solo de ese dominio cuenta predefinida de administrador como un miembro de forma predeterminada, similar al grupo de administradores de empresa. El grupo de administradores de esquema pretende que se rellenen solo temporalmente y en ocasiones (cuando se requiere la modificación del esquema de AD DS).  
  
Aunque el grupo de SA es el único grupo que se puede modificar el esquema de Active Directory (ese Salomón, estructuras de datos subyacentes del directorio como objetos y atributos), el ámbito de los derechos y permisos del grupo SA es más limitado que los grupos se ha descrito anteriormente. También es común que las organizaciones han desarrollado procedimientos adecuados para la administración de los miembros del grupo SA porque la pertenencia al grupo se suele necesitar con poca frecuencia y solo durante cortos períodos de tiempo de encontrar. Esto es técnicamente cierto de los EA, DA BA grupos y en Active Directory, así, pero es mucho menos comunes para encontrar el que las organizaciones han implementado procedimientos similares para estos grupos en el grupo de asociación de seguridad.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Cuentas protegidas y los grupos de Active Directory  
Dentro de Active Directory, un conjunto predeterminado de grupos y cuentas con privilegios denominado "protegidas" cuentas y grupos se protegen de forma diferente a otros objetos en el directorio. Cualquier cuenta que tenga directa o transitiva pertenencia a cualquier grupo protegido (independientemente de si la suscripción se deriva de los grupos de seguridad o de distribución) hereda esta seguridad restringidas.  

  
Por ejemplo, si un usuario es miembro de un grupo de distribución es decir, a su vez, un miembro de un grupo protegido en Active Directory, ese objeto de usuario se marca como una cuenta protegida. Cuando se bloquee una cuenta como una cuenta protegida, el valor del atributo adminCount en el objeto se establece en 1.  
  
> [!NOTE]
> Aunque transitiva pertenencia a un grupo protegido incluye distribución anidado y grupos de seguridad anidados, las cuentas que son miembros del grupo de distribución no recibirá a SID del grupo protegido en sus tokens de acceso. Sin embargo, los grupos de distribución se pueden convertir a grupos de seguridad de Active Directory, que es la razón por grupos de distribución se incluyen en la enumeración de miembros del grupo protegido. Debe un grupo de distribución anidado protegido nunca puede convertirse en un grupo de seguridad, las cuentas que son miembros del grupo de distribución anterior posteriormente, recibirá al elemento primario protejan el SID del grupo en sus tokens de acceso en el siguiente inicio de sesión.  
  
La siguiente tabla enumera la protegido cuentas y grupos predeterminados en Active Directory de nivel de sistema operativo versión y service pack.  
  
**Protegido predeterminada cuentas y grupos de Active Directory por sistema operativo y versión de Service Pack (SP)**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008 - Windows Server 2012**|  
|Administradores|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|  
||Administrador|Administrador|Administrador|  
||Administradores|Administradores|Administradores|  
|Administradores de dominio|Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|  
||Publicadores de certificados|||  
||Administradores de dominio|Administradores de dominio|Administradores de dominio|  
|Administradores de empresa|Controladores de dominio|Controladores de dominio|Controladores de dominio|  
||Administradores de empresa|Administradores de empresa|Administradores de empresa|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operadores de impresión|Operadores de impresión|Operadores de impresión|  
||||Controladores de dominio de solo lectura|  
||Replicator|Replicator|Replicator|  
|Administradores de esquema|Administradores de esquema||Administradores de esquema|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder y SDProp  
En el contenedor del sistema de cada dominio de Active Directory, automáticamente se crea un objeto llamado AdminSDHolder. El propósito del objeto AdminSDHolder es garantizar que los permisos de grupos y cuentas protegidas de forma coherente cumplen, independientemente de dónde se encuentran las cuentas y grupos protegidos en el dominio.  

Cada 60 minutos (predeterminada), un proceso conocido como propagador del Descriptor de seguridad (SDProp) que se ejecuta en el controlador de dominio que contiene el rol de emulador PDC del dominio. SDProp compara los permisos de objeto de AdminSDHolder del dominio con los permisos en las cuentas protegidas y los grupos en el dominio. Si los permisos en ninguno de los grupos y cuentas protegidas no coinciden con los permisos en el objeto AdminSDHolder, se restablecen los permisos de las cuentas protegidas y los grupos para que coincida con las del objeto de AdminSDHolder del dominio.  
  
La herencia de permisos está deshabilitada en grupos protegidos y cuentas, lo que significa que incluso si las cuentas o grupos se mueven a diferentes ubicaciones en el directorio, no heredan permisos de sus objetos primarios nuevo. También se deshabilita la herencia en el objeto AdminSDHolder para que los cambios en los permisos a los objetos primarios no cambian los permisos de AdminSDHolder.  
  
> [!NOTE]
> Cuando se quita una cuenta de un grupo protegido, ya no se considera una cuenta protegida, pero su permanece de atributo de adminCount establecido en 1 si no se cambia manualmente. El resultado de esta configuración es que las ACL del objeto ya no se actualizan mediante SDProp, pero el objeto todavía no hereda permisos de su objeto primario. Por lo tanto, el objeto puede residir en una unidad organizativa (OU) que delegados los permisos, pero el objeto protegido anteriormente no heredará estos permisos delegados. Un script para localizar y restablecer objetos protegidos anteriormente en el dominio puede encontrarse en la [artículo de Microsoft Support 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propiedad AdminSDHolder  
El grupo del dominio BA pertenecen la mayoría de los objetos de Active Directory. Sin embargo, el objeto AdminSDHolder es, de manera predeterminada, propiedad del grupo del dominio DA. (Este es un caso en el que DAs no se derivan sus derechos y permisos a través de la pertenencia al grupo de administradores del dominio).  
  
En las versiones de Windows anteriores a Windows Server 2008, los propietarios de un objeto pueden cambiar los permisos del objeto, incluyendo la concesión de sí mismos permisos que no originalmente tenían. Por lo tanto, los permisos predeterminados de objeto de un dominio AdminSDHolder impedir que los usuarios que son miembros del grupo BA o EA de cambiar los permisos de objeto de AdminSDHolder de un dominio. Sin embargo, los miembros del grupo Administradores del dominio pueden tomar posesión del objeto y concederse a sí mismos permisos adicionales, lo que significa que esta protección está rudimentaria y solo protege el objeto frente a modificaciones accidentales por los usuarios que no son miembros del grupo en el dominio DA. Además, los BA y EA (cuando sea aplicable) grupos tienen permiso para cambiar los atributos del objeto AdminSDHolder en el dominio local (dominio raíz para EA).  
  
> [!NOTE]  
> Un atributo en el objeto AdminSDHolder, dSHeuristics, permite la personalización limitada (eliminación) de los grupos que se consideran grupos protegidos y que se ven afectadas por AdminSDHolder y SDProp. Esta personalización debería considerarse si se implementa, aunque hay circunstancias válidas en el que la modificación de dSHeuristics en AdminSDHolder es útil. Más información sobre la modificación del atributo dSHeuristics en un objeto AdminSDHolder puede encontrarse en los artículos de Microsoft Support [817433 los](https://support.microsoft.com/?id=817433) y [973840](https://support.microsoft.com/kb/973840)y en [Apéndice C: protegido cuentas y los grupos de Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Si bien aquí se describen los grupos más con privilegios en Active Directory, hay un número de otros grupos que se han concedido elevados niveles de privilegios. Para obtener más información acerca de todos los predeterminados y los grupos integrados en Active Directory y los derechos de usuario a cada uno, consulta [Apéndice B: las cuentas con privilegios y los grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


