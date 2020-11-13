---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Recomendaciones de la directiva de auditoría
description: Trata la configuración de la Directiva de auditoría predeterminada de Windows, la configuración de directiva de auditoría recomendada de línea de base y las recomendaciones más agresivas de Microsoft para los productos de servidor y estación de trabajo.
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: debb9cf5190c5ff08a2dfd5b9e83efc16c06169d
ms.sourcegitcommit: 6a245fefdf958bfc0aeb69f7a887d11a07bdcd23
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94570340"
---
# <a name="audit-policy-recommendations"></a>Recomendaciones de la directiva de auditoría

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

En esta sección se trata la configuración de directiva de auditoría predeterminada de Windows, la configuración de directiva de auditoría recomendada de línea de base y las recomendaciones más agresivas de Microsoft para los productos de servidor y estación de trabajo.

Las recomendaciones de línea base de SCM que se muestran aquí, junto con la configuración que se recomienda para ayudar a detectar el riesgo, solo están destinadas a una guía básica de inicio para los administradores. Cada organización debe tomar sus propias decisiones con respecto a las amenazas a las que se encuentran, sus tolerancias de riesgo aceptables y qué categorías o subcategorías de directivas de auditoría deben habilitar. Para obtener más información acerca de las amenazas, consulte la [Guía de amenazas y contramedidas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125921(v=ws.10)). Se recomienda que los administradores sin una directiva de auditoría exhaustiva se empiecen con la configuración recomendada aquí y, a continuación, modificar y probar, antes de la implementación en su entorno de producción.

Las recomendaciones son para equipos de clase empresarial, que Microsoft define como equipos que tienen requisitos de seguridad medios y requieren un alto nivel de funcionalidad operativa. Las entidades que necesitan más requisitos de seguridad deberían tener en cuenta directivas de auditoría más agresivas.

> [!NOTE]
> Los valores predeterminados y las recomendaciones de línea de base de Microsoft se han tomado de la [herramienta Microsoft Security Compliance Manager](/previous-versions/tn-archive/cc677002(v=technet.10)).

Se recomienda la siguiente configuración de la Directiva de auditoría de línea de base para los equipos de seguridad normales que no se sabe que están bajo ataque activo y correcto determinando los adversarios o malware.

## <a name="recommended-audit-policies-by-operating-system"></a>Directivas de auditoría recomendadas por sistema operativo

Esta sección contiene tablas que enumeran las recomendaciones de configuración de auditoría que se aplican a los siguientes sistemas operativos:

- Windows Server 2016
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2008
- Windows 10
- Windows 8.1
- Windows 7

Estas tablas contienen la configuración predeterminada de Windows, las recomendaciones de línea base y las recomendaciones más seguras para estos sistemas operativos.

**Leyenda de tablas de directivas de auditoría**

| **Notación** | **Recomendación** |
| --- | --- |
| SÍ | Habilitar en escenarios generales |
| No | **No** habilitar en escenarios generales |
| IF | Habilitar si es necesario para un escenario concreto, o si se instala en el equipo un rol o característica para el que se desea la auditoría |
| DC | Habilitar en controladores de dominio |
| En blanco | Sin recomendación |

**Recomendaciones de configuración de auditoría de Windows 10, Windows 8 y Windows 7**

**Directiva de auditoría**

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Inicio de sesión de la cuenta** |  |  |  |
| Auditar validación de credenciales | `No  \ | No` | `Yes  \ | No` | `Yes  \ | Yes` |
| Auditar servicio de autenticación Kerberos |  |  | `Yes  \ | Yes` |
| Auditar operaciones de vales de servicio Kerberos |  |  | `Yes  \ | Yes` |
| Auditar otros eventos de inicio de sesión de cuentas |  |  | `Yes  \ | Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Administración de cuentas** |  |  |  |
| Auditar administración de grupos de aplicaciones |  |  |  |
| Auditar administración de cuentas de equipo |  | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar administración de grupos de distribución |  |  |  |
| Auditar otros eventos de administración de cuentas |  | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar administración de grupos de seguridad |  | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar administración de cuentas de usuario | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Seguimiento detallado** |  |  |  |
| Auditar actividad DPAPI |  |  | `Yes  \|  Yes` |
| Auditar creación de procesos |  | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar finalización de procesos |  |  |  |
| Auditar eventos de RPC |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Acceso DS** |  |  |  |
| Auditar replicación de servicio de directorio detallada |  |  |  |
| Auditar el acceso del servicio de directorio |  |  |  |
| Auditar cambios de servicio de directorio |  |  |  |
| Auditar replicación de servicio de directorio |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Inicio y cierre de sesión** |  |  |  |
| Auditar bloqueo de cuentas | `Yes  \|  No` |  | `Yes  \|  No` |
| Auditar reclamaciones de usuario o dispositivo |  |  |  |
| Auditar modo extendido de IPsec |  |  |  |
| Auditar modo principal de IPsec |  |  | `IF  \|  IF` |
| Auditar modo rápido de IPsec |  |  |  |
| Auditar cierre de sesión | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  No` |
| Auditar inicio de sesión <sup>1</sup> | `Yes  \|  Yes` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar Servidor de directivas de redes | `Yes  \|  Yes` |  |  |
| Auditar otros eventos de inicio y cierre de sesión |  |  |  |
| Auditar inicio de sesión especial | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Acceso a objetos** |  |  |  |
| Auditar aplicación generada |  |  |  |
| Auditar servicios de certificación |  |  |  |
| Auditar recurso compartido de archivos detallado |  |  |  |
| Auditar recurso compartido de archivos |  |  |  |
| Auditar sistema de archivos |  |  |  |
| Auditar conexión de Plataforma de filtrado |  |  |  |
| Auditar colocación de paquetes de Plataforma de filtrado |  |  |  |
| Auditar manipulación de identificadores |  |  |  |
| Auditar objeto de kernel |  |  |  |
| Auditar otros eventos de acceso a objetos |  |  |  |
| Auditar Registro |  |  |  |
| Auditar almacenamiento extraíble |  |  |  |
| Auditar SAM |  |  |  |
| Auditar almacenamiento provisional de directiva de acceso central |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Cambio de Directiva** |  |  |  |
| Auditar cambio de directiva de auditoría | `Yes  \|  No` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar cambio de directiva de autenticación | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar cambio de directiva de autorización |  |  |  |
| Auditar cambio de directiva de Plataforma de filtrado |  |  |  |
| Auditar cambio de directiva del nivel de reglas de MPSSVC |  |  | Yes |
| Auditar otros eventos de cambio de directiva |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Uso de privilegios** |  |  |  |
| Auditar uso de privilegios no confidenciales |  |  |  |
| Auditar otros eventos de uso de privilegios |  |  |  |
| Auditar uso de privilegios confidenciales |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Sistema** |  |  |  |
| Auditar controlador IPsec |  | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar otros eventos del sistema | `Yes  \|  Yes` |  |  |
| Auditar cambio de estado de seguridad | `Yes  \|  No` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar extensión del sistema de seguridad |  | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar integridad del sistema | `Yes  \|  Yes` | `Yes  \|  Yes` | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Auditoría de acceso a objetos global** |  |  |  |
| Auditar controlador IPsec |  |  |  |
| Auditar otros eventos del sistema |  |  |  |
| Auditar cambio de estado de seguridad |  |  |  |
| Auditar extensión del sistema de seguridad |  |  |  |
| Auditar integridad del sistema |  |  |  |

<sup>1</sup> a partir de la versión 1809 de Windows 10, el inicio de sesión de auditoría está habilitado de forma predeterminada tanto para correcto como para error. En versiones anteriores de Windows, solo se habilitaba correctamente de forma predeterminada.


**Recomendaciones de configuración de auditoría de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008**

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Inicio de sesión de la cuenta** |  |  |  |
| Auditar validación de credenciales | `No  \|  No` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar servicio de autenticación Kerberos |  |  | `Yes  \|  Yes` |
| Auditar operaciones de vales de servicio Kerberos |  |  | `Yes  \|  Yes` |
| Auditar otros eventos de inicio de sesión de cuentas |  |  | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Administración de cuentas** |  |  |  |
| Auditar administración de grupos de aplicaciones |  |  |  |
| Auditar administración de cuentas de equipo |  | `Yes  \|  DC` | `Yes  \|  Yes` |
| Auditar administración de grupos de distribución |  |  |  |
| Auditar otros eventos de administración de cuentas |  | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar administración de grupos de seguridad |  | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar administración de cuentas de usuario | `Yes  \|  No` | `Yes  \|  Yes` | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Seguimiento detallado** |  |  |  |
| Auditar actividad DPAPI |  |  | `Yes  \|  Yes` |
| Auditar creación de procesos |  | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar finalización de procesos |  |  |  |
| Auditar eventos de RPC |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Acceso DS** |  |  |  |
| Auditar replicación de servicio de directorio detallada |  |  |  |
| Auditar el acceso del servicio de directorio |  | `DC  \|  DC` | `DC  \|  DC` |
| Auditar cambios de servicio de directorio |  | `DC  \|  DC` | `DC  \|  DC` |
| Auditar replicación de servicio de directorio |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Inicio y cierre de sesión** |  |  |  |
| Auditar bloqueo de cuentas | `Yes  \|  No` |  | `Yes  \|  No` |
| Auditar reclamaciones de usuario o dispositivo |  |  |  |
| Auditar modo extendido de IPsec |  |  |  |
| Auditar modo principal de IPsec |  |  | `IF  \|  IF` |
| Auditar modo rápido de IPsec |  |  |  |
| Auditar cierre de sesión | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  No` |
| Auditar inicio de sesión | `Yes  \|  Yes` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar Servidor de directivas de redes | `Yes  \|  Yes` |  |  |
| Auditar otros eventos de inicio y cierre de sesión |  |  | `Yes  \|  Yes` |
| Auditar inicio de sesión especial | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Acceso a objetos** |  |  |  |
| Auditar aplicación generada |  |  |  |
| Auditar servicios de certificación |  |  |  |
| Auditar recurso compartido de archivos detallado |  |  |  |
| Auditar recurso compartido de archivos |  |  |  |
| Auditar sistema de archivos |  |  |  |
| Auditar conexión de Plataforma de filtrado |  |  |  |
| Auditar colocación de paquetes de Plataforma de filtrado |  |  |  |
| Auditar manipulación de identificadores |  |  |  |
| Auditar objeto de kernel |  |  |  |
| Auditar otros eventos de acceso a objetos |  |  |  |
| Auditar Registro |  |  |  |
| Auditar almacenamiento extraíble |  |  |  |
| Auditar SAM |  |  |  |
| Auditar almacenamiento provisional de directiva de acceso central |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Cambio de Directiva** |  |  |  |
| Auditar cambio de directiva de auditoría | `Yes  \|  No` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar cambio de directiva de autenticación | `Yes  \|  No` | `Yes  \|  No` | `Yes  \|  Yes` |
| Auditar cambio de directiva de autorización |  |  |  |
| Auditar cambio de directiva de Plataforma de filtrado |  |  |  |
| Auditar cambio de directiva del nivel de reglas de MPSSVC |  |  | Yes |
| Auditar otros eventos de cambio de directiva |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Uso de privilegios** |  |  |  |
| Auditar uso de privilegios no confidenciales |  |  |  |
| Auditar otros eventos de uso de privilegios |  |  |  |
| Auditar uso de privilegios confidenciales |  |  |  |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Sistema** |  |  |  |
| Auditar controlador IPsec |  | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar otros eventos del sistema | `Yes  \|  Yes` |  |  |
| Auditar cambio de estado de seguridad | `Yes  \|  No` | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar extensión del sistema de seguridad |  | `Yes  \|  Yes` | `Yes  \|  Yes` |
| Auditar integridad del sistema | `Yes  \|  Yes` | `Yes  \|  Yes` | `Yes  \|  Yes` |

| Categoría o subcategoría de directiva de auditoría | Predeterminado de Windows<p>Realizado | Fallida | Recomendación de línea de base<p>Realizado | Fallida | Recomendación más fuerte<p>Realizado | Fallida |
| --- | --- | --- | --- |
| **Auditoría de acceso a objetos global** |  |  |  |
| Auditar controlador IPsec |  |  |  |
| Auditar otros eventos del sistema |  |  |  |
| Auditar cambio de estado de seguridad |  |  |  |
| Auditar extensión del sistema de seguridad |  |  |  |
| Auditar integridad del sistema |  |  |  |

## <a name="set-audit-policy-on-workstations-and-servers"></a>Establecer la Directiva de auditoría en estaciones de trabajo y servidores

Todos los planes de administración del registro de eventos deben supervisar estaciones de trabajo y servidores. Un error común es supervisar solo servidores o controladores de dominio. Dado que la piratería malintencionada suele producirse en las estaciones de trabajo, no la supervisión de las estaciones de trabajo omite la mejor fuente de información y la más temprana.

Los administradores deben revisar y probar atentamente cualquier directiva de auditoría antes de la implementación en su entorno de producción.

## <a name="events-to-monitor"></a>Eventos para supervisar

Un identificador de evento perfecto para generar una alerta de seguridad debe contener los siguientes atributos:

- Alta probabilidad de que la ocurrencia indique una actividad no autorizada

- Deben disponer de un número reducido de falsos positivos.

- La repetición debe dar como resultado una respuesta de investigación/análisis forense

Se deben supervisar y alertar dos tipos de eventos:

1. Los eventos en los que incluso una única repetición indica actividad no autorizada.

2. Una acumulación de eventos por encima de un punto de referencia esperado y aceptado.

Un ejemplo del primer evento es:

Si los administradores de dominio (DAs) no pueden iniciar sesión en equipos que no son controladores de dominio, una única repetición de un miembro de DA que inicie sesión en una estación de trabajo de usuario final debe generar una alerta y ser investigada. Este tipo de alerta es fácil de generar mediante el evento de inicio de sesión especial Audit 4964 (los grupos especiales se han asignado a un nuevo inicio de sesión). Otros ejemplos de alertas de instancia única incluyen:

- Si el servidor a nunca se debe conectar al servidor B, avise cuando se conecte entre sí.

- Alerta si una cuenta de usuario final normal se agrega de forma inesperada a un grupo de seguridad confidencial.

- Si los empleados de la ubicación de fábrica nunca funcionan en la noche, alerta cuando un usuario inicia sesión a medianoche.

- Alerta si se ha instalado un servicio no autorizado en un controlador de dominio.

- Investigue si un usuario final normal intenta iniciar sesión directamente en una SQL Server para la que no tiene ningún motivo claro para hacerlo.

- Si no tiene ningún miembro en el grupo de DA y alguien lo agrega a sí mismo, compruébelo inmediatamente.

Un ejemplo del segundo evento es:

Un número Aberrant de inicios de sesión erróneos podría indicar un ataque de adivinación de contraseñas. Para que una empresa proporcione una alerta para un número inusualmente alto de inicios de sesión con error, primero debe comprender los niveles normales de inicios de sesión con error dentro de su entorno antes de un evento de seguridad malintencionado.

Para obtener una lista completa de los eventos que se deben incluir al supervisar los síntomas de riesgo, consulte [Apéndice L: eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Active Directory objetos y atributos que se van a supervisar

A continuación se muestran las cuentas, los grupos y los atributos que debe supervisar para ayudarle a detectar los intentos de poner en peligro la instalación de Active Directory Domain Services.

- Sistemas para deshabilitar o quitar software antivirus y antimalware (reiniciar automáticamente la protección cuando se deshabilita manualmente)

- Cuentas de administrador para cambios no autorizados

- Las actividades que se realizan mediante cuentas con privilegios (quita automáticamente la cuenta cuando se completan las actividades sospechosas o el tiempo asignado ha expirado)

- Cuentas con privilegios y VIP en AD DS. Supervise los cambios, especialmente los cambios en los atributos de la pestaña cuenta (por ejemplo, CN, Name, sAMAccountName, userPrincipalName o userAccountControl). Además de supervisar las cuentas, restrinja quién puede modificar las cuentas como un pequeño conjunto de usuarios administrativos como sea posible.

Consulte [Apéndice L: events to Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obtener una lista de los eventos recomendados que se deben supervisar, su clasificación de importancia y un resumen de mensajes de eventos.

- Agrupar servidores por la clasificación de sus cargas de trabajo, lo que le permite identificar rápidamente los servidores que deben ser los más supervisados y configurados de forma más rigurosa

- Cambios en las propiedades y pertenencia a los siguientes grupos de AD DS: administradores de empresas (EA), Admins. del dominio (DA), administradores (BA) y administradores de esquema (SA)

- Cuentas con privilegios deshabilitadas (como las cuentas de administrador integradas en Active Directory y en los sistemas miembro) para habilitar las cuentas

- Cuentas de administración para registrar todas las escrituras en la cuenta

- Asistente para configuración de seguridad integrado para configurar el servicio, el registro, la auditoría y la configuración del firewall para reducir la superficie expuesta a ataques del servidor. Use este asistente si implementa servidores de saltos como parte de su estrategia de hosts administrativos.

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Información adicional para la supervisión de Active Directory Domain Services

Revise los siguientes vínculos para obtener información adicional sobre la supervisión de AD DS:

- La [Auditoría de acceso a objetos global es mágica](/archive/blogs/askds/global-object-access-auditing-is-magic) : proporciona información sobre la configuración y el uso de la configuración de directivas de auditoría avanzada que se agregó a Windows 7 y windows Server 2008 R2.

- [Introducción a los cambios de auditoría en windows 2008](/archive/blogs/askds/introducing-auditing-changes-in-windows-2008) : presenta los cambios de auditoría realizados en Windows 2008.

- [Trucos de auditoría fantásticos en vista y 2008](/archive/blogs/askds/cool-auditing-tricks-in-vista-and-2008) : se explican las nuevas características interesantes de la auditoría en Windows Vista y windows Server 2008 que se pueden usar para solucionar problemas o ver lo que sucede en su entorno.

- [Tienda única para la auditoría en Windows server 2008 y Windows Vista](/archive/blogs/askds/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista) : contiene una compilación de características de auditoría e información contenida en windows Server 2008 y Windows Vista.

- [AD DS guía paso a paso de auditoría](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731607(v=ws.10)) : describe la nueva característica de auditoría de Active Directory Domain Services (AD DS) en Windows Server 2008. También se proporcionan procedimientos para implementar esta nueva característica.

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Lista general de críticas de recomendaciones de ID. de eventos de seguridad

Todas las recomendaciones de ID. de evento van acompañadas de una clasificación de importancia crítica de la siguiente manera:

**Alto:** Los identificadores de eventos con una clasificación de nivel de importancia alta siempre deben alertarse e investigarse inmediatamente.

**Medio:** Un ID. de evento con una clasificación de gravedad media podría indicar actividad malintencionada, pero debe ir acompañado de otra anomalía (por ejemplo, un número inusual en un período de tiempo determinado, repeticiones inesperadas o repeticiones de un equipo que normalmente no se esperarían registrar el evento). También se puede recopilar un evento de importancia media y compararlo con el tiempo.

**Bajo:** Y el ID. de evento con un bajo nivel de gravedad no deben obtener atención ni producir alertas, a menos que estén correlacionados con eventos de importancia media o alta.

Estas recomendaciones están pensadas para proporcionar una guía de línea de base para el administrador. Todas las recomendaciones deben revisarse minuciosamente antes de la implementación en un entorno de producción.

Consulte [Apéndice L: events to Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obtener una lista de los eventos recomendados que se deben supervisar, su clasificación de importancia y un resumen de mensajes de eventos.
