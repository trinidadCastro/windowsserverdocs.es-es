---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Namespace desligada
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac231dbfbaaeafa39199e29a1744d84d5cec23e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="disjoint-namespace"></a>Namespace desligada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Espacio de nombres discontinuo se produce cuando uno o más equipos de miembro de dominio tienen un sufijo principal del servicio de nombres de dominio (DNS) que no coincide con el nombre DNS del dominio de Active Directory de que los equipos son miembros. Por ejemplo, un equipo miembro que usa un sufijo DNS principal de corp.fabrikam.com en un dominio de Active Directory denominado na.corp.fabrikam.com está usando un espacio de nombres desligada.  
  
Espacio de nombres discontinuo es más difícil de administrar, mantener y solucionar problemas de un espacio de nombres contiguo. En un espacio de nombres contiguo el sufijo DNS principal coincide con el nombre de dominio de Active Directory. Las aplicaciones de red que se escriben asumir que el espacio de nombres de Active Directory es idéntico al sufijo DNS principal para todos los equipos miembro de dominio no funcionan correctamente en un espacio de nombres desligada.  
  
## <a name="support-for-disjoint-namespaces"></a>Soporte técnico para desligadas espacios de nombres  
Equipos miembros del dominio, incluidos los controladores de dominio, pueden funcionar en un espacio de nombres desligada. Equipos miembros del dominio pueden registrar su host (A) IP versión 6 (IPv6) y registro de recursos hospedar registro de recursos (AAAA) en un espacio de nombres DNS desligada. Cuando los equipos miembros del dominio registran sus registros de recursos de esta forma, los controladores de dominio seguirán registrar los registros de recursos específicos del sitio y global de servicio (SRV) en la zona DNS que es idéntica al nombre de dominio de Active Directory.  
  
Por ejemplo, supongamos que un controlador de dominio para el dominio de Active Directory denominado na.corp.fabrikam.com que usa un sufijo DNS principal de corp.fabrikam.com registra host (A) y registros de recursos de IPv6 (AAAA) del host en la zona DNS corp.fabrikam.com. El controlador de dominio continúa registrar los registros de recursos específicos del sitio y global de servicio (SRV) en las _msdcs. na.corp.fabrikam.com y na.corp.fabrikam.com zonas DNS, lo que hace posible la ubicación del servicio.  
  
> [!IMPORTANT]  
> Aunque los sistemas operativos Windows puede admitir un espacio de nombres discontinuo, las aplicaciones que se escriben en asumir que el sufijo DNS principal es el mismo que el sufijo de dominio de Active Directory no funcionen en este entorno. Por este motivo, debes probar todas las aplicaciones y sus respectivos sistemas operativos cuidadosamente antes de implementar un espacio de nombres desligada.  
  
Espacio de nombres discontinuo debería funcionar (y es compatible) en las siguientes situaciones:  
  
-   Cuando un bosque de varios dominios de Active Directory usa un espacio de nombres DNS, que también se conoce como una zona DNS  
  
    Un ejemplo de esto es una empresa que usa dominios regionales con nombres como na.corp.fabrikam.com, sa.corp.fabrikam.com y asia.corp.fabrikam.com y usa un único espacio de nombres DNS, como corp.fabrikam.com.  
  
-   Cuando un solo dominio de Active Directory se divide en espacios de nombres independientes de DNS  
  
    Un ejemplo de esto es una empresa con un dominio de Active Directory de corp.contoso.com que usa las zonas DNS como hr.corp.contoso.com, production.corp.contoso.com y it.corp.contoso.com.  
  
Un espacio de nombres discontinuo no funciona correctamente (y no se admite) en las siguientes situaciones:  
  
-   Un sufijo desligado utilizado por los miembros del dominio coincide con un nombre de dominio de Active Directory en el bosque de este u otro. Esto saltos de enrutamiento de sufijo del nombre de Kerberos.  
  
-   Se usa el mismo sufijo desligado de otro bosque. Esto impide que el enrutamiento de forma exclusiva estos sufijos entre bosques.  
  
-   Cuando una entidad de certificación de miembro de dominio (CA) cambios del servidor, es totalmente nombre de dominio completo (FQDN) para que ya no uses el mismo sufijo DNS principal que se usa en los controladores de dominio del dominio al que el servidor de CA es un miembro. En este caso, puede que problemas de validación de certificados emitido, dependiendo de los nombres de DNS se usan en los puntos de distribución CRL el servidor de CA. Sin embargo, si un servidor de CA se coloca en un espacio de nombres discontinuo estable, funciona correctamente y que es compatible.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Consideraciones para desligadas espacios de nombres  
Las consideraciones siguientes pueden ayudarte a decidir si debes usar un espacio de nombres desligada.  
  
### <a name="application-compatibility"></a>Compatibilidad de aplicaciones  
Como se mencionó anteriormente, un espacio de nombres discontinuo puede provocar problemas para las aplicaciones y los servicios que se escriben en supongamos que un sufijo DNS principal de equipo es idéntico al nombre del nombre de dominio de las cuales es un miembro. Antes de implementar un espacio de nombres discontinuo, debes comprobar aplicaciones para los problemas de compatibilidad. Además, asegúrate de comprobar la compatibilidad de todas las aplicaciones que usas al realizar el análisis. Esto incluye aplicaciones de Microsoft y de otros desarrolladores de software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Ventajas de espacios de nombres desligadas  
Uso de un espacio de nombres discontinuo puede tener las siguientes ventajas:  
  
-   Porque el sufijo DNS principal de un equipo puede indicar información diferente, puedes administrar el espacio de nombres DNS por separado desde el nombre de dominio de Active Directory.  
  
-   Puedes separar el espacio de nombres DNS en función de la estructura de la empresa o la ubicación geográfica. Por ejemplo, puedes separar el espacio de nombres en función de los nombres de unidades de negocio o ubicación física como continente, país o región o edificio.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Desventajas de espacios de nombres desligadas  
Uso de un espacio de nombres discontinuo puede tener las siguientes desventajas:  
  
-   Debes crear y administrar las zonas DNS independientes para cada dominio de Active Directory en el bosque que tiene equipos miembro que usan un espacio de nombres desligada. (Es decir, requiere una configuración adicional y más compleja).  
  
-   Debes realizar pasos manuales para modificar y administrar el atributo de Active Directory que permite a los miembros del dominio usan sufijos DNS principales, especificados.  
  
-   Para optimizar la resolución de nombres, debes realizar pasos manuales para modificar y mantener directivas de grupo para configurar los equipos miembro con alternativos sufijos DNS principales.  
  
    > [!NOTE]  
    > El servicio de nombres de Windows Internet (WINS) puede usarse para compensar esta desventaja mediante la resolución de nombres de etiqueta única. Para obtener más información acerca de WINS, consulta la referencia técnica de WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Cuando el entorno requiere varios sufijos DNS principales, debes configurar el orden de búsqueda de sufijo DNS para todos los dominios de Active Directory en el bosque correctamente.  
  
    Para establecer el orden de búsqueda de sufijo DNS, puedes usar parámetros de servicios de servidor de protocolo de configuración dinámica de Host (DHCP) o los objetos de directiva de grupo. También puedes modificar el registro.  
  
-   Debes probar cuidadosamente todas las aplicaciones para los problemas de compatibilidad.  
  
Para obtener más información acerca de los pasos que puede seguir para solucionar estas desventajas, consulta crear una desligadas Namespace ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Planear una transición del espacio de nombres  
Antes de modificar un espacio de nombres, consulte las siguientes consideraciones, que se aplican a las transiciones desde los espacios de nombres contiguos discontinuas espacios de nombres (o viceversa):  
  
-   Configurar manualmente los nombres de entidad de seguridad de servicio (SPN) puede ya no coinciden con nombres DNS después de un cambio de espacio de nombres. Esto puede provocar errores de autenticación.  
  
    Para obtener más información, consulta el tema servicio inicios de sesión de un error debido a establecer SPN incorrectamente ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Si usas equipos basados en Windows Server 2003 con la delegación restringida, esos equipos pueden requerir una configuración adicional para cambiar el SPN. Para obtener más información, consulta el artículo 936628 en Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Si desea delegar permisos para modificar el SPN para subordinadas los administradores, consulta delegar autoridad para modificar SPN ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Si usas Protocolo ligero de acceso a directorios (LDAP) sobre Sockets capa seguros (SSL) (conocido como LDAPS) con una entidad de certificación en una implementación que tiene los controladores de dominio que se configuran en un espacio de nombres discontinuo, debes usar el nombre de dominio de Active Directory apropiado y el sufijo DNS principal cuando se configura los certificados LDAPS.  
  
    Para obtener más información sobre los requisitos de certificado de controlador de dominio, consulta el artículo 321051 en Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > Controladores de dominio que usen certificados para LDAPS pueden requerir que se va a implementar los certificados. Al hacerlo, los controladores de dominio no pueden seleccionar un certificado apropiado hasta que se reinicien. Para obtener más información sobre la autenticación de LDAPS y relacionadas con una actualización de Windows Server 2003, consulta el artículo 932834 en Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Planeamiento de implementaciones de espacio de nombres discontinuo  
Si implementas equipos en un entorno que tenga un espacio de nombres discontinuo, adopta las precauciones siguientes:  
  
1.  Notificar a todos los proveedores de software con la que hace negocio que deben probar y admiten un espacio de nombres desligada. Pídele que compruebe que admiten las aplicaciones en entornos que usan los espacios de nombres desligadas.  
  
2.  Prueba todas las versiones de sistemas operativos y aplicaciones en entornos de laboratorio de espacio de nombres discontinuo. Al hacerlo, sigue estas recomendaciones:  
  
    1.  Resolver todos los problemas de software antes de implementar el software en tu entorno.  
  
    2.  Cuando sea posible, participar en pruebas beta de sistemas operativos y aplicaciones que vas a implementar en espacios de nombres desligadas.  
  
3.  Asegúrate de que los administradores y servicio de asistencia personal conozcan el espacio de nombres discontinuo y su impacto.  
  
4.  Crear un plan que hace posible que puedes realizar una transición de un espacio de nombres desligada un espacio de nombres contiguo, si es necesario.  
  
5.  Evangeliza la importancia del espacio de nombres discontinuo compatibilidad con el sistema operativo y los proveedores de la aplicación.  
  


