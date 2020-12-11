---
description: Más información acerca de cómo reducir la superficie expuesta a ataques de Active Directory
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Reducción de la superficie de ataque de Active Directory
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0486f62f2c53c427a196cd6e68a8b879c63df2ab
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040633"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Reducción de la superficie de ataque de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta sección se centra en los controles técnicos que se deben implementar para reducir la superficie expuesta a ataques de la instalación de Active Directory. La sección contiene la siguiente información:

- La [implementación de Least-Privilege modelos administrativos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) se centra en identificar el riesgo que supone el uso de cuentas con privilegios elevados para la administración diaria, además de proporcionar recomendaciones para reducir el riesgo de que se presenten las cuentas con privilegios.

- [Implementar hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) describe los principios para la implementación de sistemas administrativos dedicados y seguros, además de algunos enfoques de ejemplo para una implementación de host administrativo segura.

- [Protección de controladores de dominio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) describe las directivas y la configuración que, aunque son similares a las recomendaciones para la implementación de hosts administrativos seguros, contienen algunas recomendaciones específicas del controlador de dominio para garantizar que los controladores de dominio y los sistemas utilizados para administrarlos estén bien protegidos.

## <a name="privileged-accounts-and-groups-in-active-directory"></a>Cuentas con privilegios y grupos de Active Directory
En esta sección se proporciona información general acerca de los grupos y las cuentas con privilegios en Active Directory diseñado para explicar el puntos y las diferencias entre las cuentas y los grupos con privilegios en Active Directory. Al comprender estas distinciones, tanto si implementa las recomendaciones en la [implementación de Least-Privilege modelos administrativos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) literalmente como si decide personalizarlos para su organización, tiene las herramientas que necesita para proteger cada grupo y cuenta de forma adecuada.

### <a name="built-in-privileged-accounts-and-groups"></a>Grupos y cuentas con privilegios integrados
Active Directory facilita la delegación de la administración y admite el principio de privilegios mínimos en la asignación de derechos y permisos. De forma predeterminada, los usuarios "normales" que tienen cuentas en un dominio pueden leer gran parte de lo que se almacena en el directorio, pero solo pueden cambiar un conjunto de datos muy limitado en el directorio. A los usuarios que requieren privilegios adicionales se les puede conceder la pertenencia a varios grupos "privilegiados" que están integrados en el directorio para que puedan realizar tareas específicas relacionadas con sus roles, pero no pueden realizar tareas que no sean relevantes para sus tareas. Las organizaciones también pueden crear grupos que se adapten a responsabilidades de trabajo específicas y se les concedan derechos y permisos granulares que permitan al personal de TI realizar funciones administrativas cotidianas sin conceder derechos y permisos que superen lo que se requiere para esas funciones.

Dentro de Active Directory, tres grupos integrados son los grupos de privilegios más altos del directorio: administradores de organización, Admins. del dominio y administradores. La configuración predeterminada y las capacidades de cada uno de estos grupos se describen en las siguientes secciones:

#### <a name="highest-privilege-groups-in-active-directory"></a>Grupos de privilegios más altos en Active Directory

##### <a name="enterprise-admins"></a>Administradores de empresas
Administradores de empresas (EA) es un grupo que solo existe en el dominio raíz del bosque y, de forma predeterminada, es miembro del grupo administradores en todos los dominios del bosque. La cuenta predefinida Administrador del dominio raíz del bosque es el único miembro predeterminado del grupo EA. A EAs se le conceden derechos y permisos que les permiten implementar los cambios en todo el bosque (es decir, los cambios que afectan a todos los dominios del bosque), como agregar o quitar dominios, establecer confianzas de bosque o elevar los niveles funcionales del bosque. En un modelo de delegación correctamente diseñado e implementado, la pertenencia a EA solo es necesaria cuando se construye por primera vez el bosque o cuando se realizan ciertos cambios en todo el bosque, como el establecimiento de una confianza de bosque de salida. La mayoría de los derechos y permisos concedidos al grupo EA se pueden delegar a usuarios y grupos con menos privilegios.

##### <a name="domain-admins"></a>Administradores de dominio

Cada dominio de un bosque tiene su propio grupo Admins. del dominio (DA), que es miembro del grupo de administradores de ese dominio y un miembro del grupo de administradores locales en todos los equipos Unidos al dominio. El único miembro predeterminado del grupo de DA para un dominio es la cuenta de administrador integrada para ese dominio. Los DAs son "todos muy eficaces" dentro de sus dominios, mientras que EAs tiene privilegios en todo el bosque. En un modelo de delegación correctamente diseñado e implementado, la pertenencia a Admins. del dominio solo debe ser necesaria en escenarios de "interrupción" (por ejemplo, situaciones en las que se necesita una cuenta con altos niveles de privilegios en todos los equipos del dominio). Aunque los mecanismos de delegación de Active Directory nativa permiten la delegación hasta el punto en el que es posible usar cuentas de DA solo en escenarios de emergencia, la creación de un modelo de delegación eficaz puede llevar mucho tiempo y muchas organizaciones aprovechan herramientas de terceros para acelerar el proceso.

##### <a name="administrators"></a>Administradores
El tercer grupo es el grupo de administradores locales de dominio (BA) integrado en el que se anidan DAs y EAs. A este grupo se le conceden muchos de los derechos y permisos directos en el directorio y en los controladores de dominio. Sin embargo, el grupo de administradores de un dominio no tiene privilegios en los servidores miembro o en las estaciones de trabajo. A través de la pertenencia al grupo de administradores locales de los equipos, se concede el privilegio local.

> [!NOTE]
> Aunque estas son las configuraciones predeterminadas de estos grupos con privilegios, un miembro de cualquiera de los tres grupos puede manipular el directorio para obtener la pertenencia a los demás grupos. En algunos casos, es trivial obtener la pertenencia a los demás grupos, mientras que en otros es más difícil, pero desde la perspectiva del posible privilegio, los tres grupos deben considerarse realmente equivalentes.

##### <a name="schema-admins"></a>Administradores de esquema

Un cuarto grupo con privilegios, administradores de esquemas (SA), solo existe en el dominio raíz del bosque y solo tiene la cuenta de administrador integrada de ese dominio como miembro predeterminado, similar al grupo administradores de empresas. El grupo administradores de esquema se ha diseñado para que se rellene solo de forma temporal y ocasionalmente (cuando se requiere la modificación del esquema AD DS).

Aunque el grupo SA es el único grupo que puede modificar el esquema de Active Directory (es decir, las estructuras de datos subyacentes del directorio como objetos y atributos), el ámbito de los derechos y permisos del grupo SA es más limitado que los grupos descritos anteriormente. También es común encontrar que las organizaciones hayan desarrollado prácticas adecuadas para la administración de la pertenencia del grupo SA, ya que la pertenencia al grupo suele ser necesaria con poca frecuencia y solo durante breves períodos de tiempo. Esto es técnicamente cierto en los grupos EA, DA y BA en Active Directory, pero es mucho menos frecuente encontrar que las organizaciones han implementado prácticas similares para estos grupos como para el grupo SA.

#### <a name="protected-accounts-and-groups-in-active-directory"></a>Cuentas protegidas y grupos de Active Directory
Dentro de Active Directory, un conjunto predeterminado de cuentas y grupos con privilegios denominados cuentas y grupos "protegidos" se protegen de forma diferente a otros objetos del directorio. Cualquier cuenta que tenga pertenencia directa o transitiva en cualquier grupo protegido (independientemente de si la pertenencia se deriva de grupos de seguridad o distribución) hereda esta seguridad restringida.


Por ejemplo, si un usuario es miembro de un grupo de distribución que, a su vez, es miembro de un grupo protegido en Active Directory, ese objeto de usuario se marca como una cuenta protegida. Cuando una cuenta se marca como una cuenta protegida, el valor del atributo adminCount del objeto se establece en 1.

> [!NOTE]
> Aunque la pertenencia transitiva de un grupo protegido incluye una distribución anidada y grupos de seguridad anidados, las cuentas que son miembros de grupos de distribución anidados no recibirán el SID del grupo protegido en sus tokens de acceso. Sin embargo, los grupos de distribución se pueden convertir en grupos de seguridad en Active Directory, por lo que los grupos de distribución se incluyen en la enumeración de miembros de grupo protegidos. En caso de que un grupo de distribución anidado protegido se convierta en un grupo de seguridad, las cuentas que son miembros del grupo de distribución anterior recibirán posteriormente el SID del grupo protegido primario en sus tokens de acceso en el siguiente inicio de sesión.

En la tabla siguiente se enumeran las cuentas y los grupos protegidos predeterminados de Active Directory por versión de sistema operativo y nivel de Service Pack.

**Cuentas y grupos protegidos predeterminados en Active Directory por sistema operativo y versión de Service Pack (SP)**

|**Windows 2000 <SP4**|**Windows 2000 SP4: Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008: Windows Server 2012**|
|--|--|--|--|
|Administradores|Operadores de cuentas|Operadores de cuentas|Operadores de cuentas|
||Administrador|Administrador|Administrador|
||Administradores|Administradores|Administradores|
|Administradores de dominio|Operadores de copia de seguridad|Operadores de copia de seguridad|Operadores de copia de seguridad|
||Publicadores de certificados|||
||Administradores de dominio|Administradores de dominio|Administradores de dominio|
|Administradores de empresas|Controladores de dominio|Controladores de dominio|Controladores de dominio|
||Administradores de empresas|Administradores de empresas|Administradores de empresas|
||Krbtgt|Krbtgt|Krbtgt|
||Operadores de impresión|Operadores de impresión|Operadores de impresión|
||||Controladores de dominio de solo lectura|
||Duplicadores|Duplicadores|Duplicadores|
|Administradores de esquema|Administradores de esquema||Administradores de esquema|

##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder y SDProp
En el contenedor del sistema de cada dominio de Active Directory, se crea automáticamente un objeto denominado AdminSDHolder. El propósito del objeto AdminSDHolder es asegurarse de que los permisos de las cuentas y grupos protegidos se aplican de forma coherente, independientemente de dónde se encuentren los grupos y las cuentas protegidos en el dominio.

Cada 60 minutos (de forma predeterminada), un proceso conocido como propagador de descriptores de seguridad (SDProp) se ejecuta en el controlador de dominio que contiene el rol de emulador de PDC del dominio. SDProp compara los permisos del objeto AdminSDHolder del dominio con los permisos en las cuentas protegidas y los grupos del dominio. Si los permisos de cualquiera de las cuentas y grupos protegidos no coinciden con los permisos en el objeto AdminSDHolder, se restablecen los permisos de los grupos y las cuentas protegidas para que coincidan con los del objeto AdminSDHolder del dominio.

La herencia de permisos está deshabilitada en grupos y cuentas protegidos, lo que significa que incluso si las cuentas o grupos se mueven a ubicaciones diferentes del directorio, no heredan los permisos de los nuevos objetos primarios. La herencia también está deshabilitada en el objeto AdminSDHolder para que los cambios de permisos en los objetos primarios no cambien los permisos de AdminSDHolder.

> [!NOTE]
> Cuando se quita una cuenta de un grupo protegido, ya no se considera una cuenta protegida, pero su atributo adminCount permanece establecido en 1 si no se cambia manualmente. El resultado de esta configuración es que SDProp ya no actualiza las ACL del objeto, pero el objeto todavía no hereda los permisos de su objeto primario. Por lo tanto, el objeto puede residir en una unidad organizativa (OU) a la que se hayan delegado los permisos, pero el objeto protegido anteriormente no heredará estos permisos delegados. Puede encontrar un script para buscar y restablecer objetos protegidos anteriormente en el dominio en el [soporte técnico de Microsoft artículo 817433](https://support.microsoft.com/?id=817433).

###### <a name="adminsdholder-ownership"></a>Propiedad AdminSDHolder
La mayoría de los objetos de Active Directory pertenecen al grupo de BA del dominio. Sin embargo, el objeto AdminSDHolder es, de forma predeterminada, propiedad del grupo de DA del dominio. (Este es un caso en el que DAs no deriva sus derechos y permisos a través de la pertenencia al grupo administradores del dominio).

En las versiones de Windows anteriores a Windows Server 2008, los propietarios de un objeto pueden cambiar los permisos del objeto, incluida la concesión de los mismos permisos que no tenían originalmente. Por lo tanto, los permisos predeterminados en el objeto AdminSDHolder de un dominio impiden que los usuarios que son miembros de los grupos BA o EA cambien los permisos del objeto AdminSDHolder de un dominio. Sin embargo, los miembros del grupo administradores del dominio pueden tomar posesión del objeto y concederse permisos adicionales, lo que significa que esta protección es rudimentaria y solo protege el objeto contra la modificación accidental por parte de usuarios que no son miembros del grupo DA del dominio. Además, los grupos BA y EA (cuando corresponda) tienen permiso para cambiar los atributos del objeto AdminSDHolder en el dominio local (dominio raíz para EA).

> [!NOTE]
> Un atributo del objeto AdminSDHolder, dSHeuristics, permite una personalización limitada (desinstalación) de grupos que se consideran grupos protegidos y que se ven afectados por AdminSDHolder y SDProp. Esta personalización debe considerarse detenidamente si se implementa, aunque hay circunstancias válidas en las que la modificación de dSHeuristics en AdminSDHolder es útil. Puede encontrar más información sobre la modificación del atributo dSHeuristics en un objeto AdminSDHolder en los Soporte técnico de Microsoft artículos [817433](https://support.microsoft.com/?id=817433) y [973840](https://support.microsoft.com/kb/973840)y en el [Apéndice C: cuentas protegidas y grupos en Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).

Aunque los grupos con más privilegios en Active Directory se describen aquí, hay varios grupos a los que se les han concedido privilegios elevados. Para obtener más información acerca de todos los grupos predeterminados e integrados en Active Directory y los derechos de usuario asignados a cada uno, vea el [Apéndice B: cuentas y grupos con privilegios en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).
