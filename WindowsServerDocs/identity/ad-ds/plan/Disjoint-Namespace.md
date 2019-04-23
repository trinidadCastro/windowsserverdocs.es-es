---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Espacio de nombres separado
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a2e911e889343b05a515c94e615d3648289f2df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883576"
---
# <a name="disjoint-namespace"></a>Espacio de nombres separado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un espacio de nombres separado se produce cuando uno o más equipos miembros del dominio tienen un sufijo de servicio de nombres de dominio (DNS) principal que no coincide con el nombre DNS del dominio de Active Directory de los cuales los equipos son miembros. Por ejemplo, un equipo miembro que usa un sufijo DNS primario corp.fabrikam.com en un dominio de Active Directory denominado na.corp.fabrikam.com, está usando un espacio de nombres no contiguo.  
  
Un espacio de nombres separado es más compleja para administrar, mantener y solucionar problemas de un espacio de nombres contiguo. En un espacio de nombres contiguo, el sufijo DNS principal coincide con el nombre de dominio de Active Directory. Aplicaciones de red que están escritas para suponer que el espacio de nombres de Active Directory es idéntico al sufijo DNS principal para todos los equipos miembros del dominio no funcionan correctamente en un espacio de nombres no contiguo.  
  
## <a name="support-for-disjoint-namespaces"></a>Compatibilidad con los espacios de nombres  
Equipos miembros del dominio, incluidos los controladores de dominio, pueden funcionar en un espacio de nombres no contiguo. Equipos miembros del dominio pueden registrar su host de registro de recursos (AAAA) en un espacio de nombres DNS separado de host (A) registro de recursos e IP versión 6 (IPv6). Cuando los equipos miembros del dominio registran sus registros de recursos de esta manera, los controladores de dominio seguirán registrar registros de recursos globales y específicas de servicios (SRV) en la zona DNS que es idéntica al nombre de dominio de Active Directory.  
  
Por ejemplo, suponga que un controlador de dominio para el dominio de Active Directory denominado na.corp.fabrikam.com, que usa el sufijo DNS primario Corp.fabrikam.com registra host (A) y registros de recursos de IPv6 (AAAA) de host en la zona DNS corp.fabrikam.com. El controlador de dominio continúa registrar registros de recursos globales y específicas de servicios (SRV) en las zonas DNS _msdcs.na.corp.fabrikam.com y na.corp.fabrikam.com, lo que hace posible a la ubicación del servicio.  
  
> [!IMPORTANT]  
> Aunque los sistemas operativos de Windows puede admitir un espacio de nombres no contiguo, las aplicaciones que se escriben en suponer que el sufijo DNS principal es el mismo que el sufijo de dominio de Active Directory pueden no funcionar en un entorno. Por este motivo, debe probar todas las aplicaciones y sus respectivos sistemas operativos con cuidado antes de implementar un espacio de nombres no contiguo.  
  
En las situaciones siguientes un espacio de nombres no contiguo debería funcionar (y es compatible):  
  
-   Cuando un bosque con varios dominios de Active Directory utiliza un espacio de nombres DNS único, que también se conoce como una zona DNS  
  
    Un ejemplo de esto es una empresa que usa dominios regionales con nombres como na.corp.fabrikam.com, sa.corp.fabrikam.com y asia.corp.fabrikam.com y usa un único espacio de nombres DNS, como corp.fabrikam.com.  
  
-   Cuando se divide un único dominio de Active Directory en espacios de nombres DNS independientes  
  
    Un ejemplo de esto es una empresa con un dominio de Active Directory de corp.contoso.com que usa zonas de DNS como hr.corp.contoso.com, production.corp.contoso.com y it.corp.contoso.com.  
  
En las situaciones siguientes un espacio de nombres no contiguo no funciona correctamente (y no se admite):  
  
-   Un sufijo separado utilizado por los miembros del dominio coincide con un nombre de dominio de Active Directory en este u otro bosque. Esto interrumpe el enrutamiento de sufijo de nombre de Kerberos.  
  
-   Se usa el mismo sufijo separado en otro bosque. Esto evita que el enrutamiento de forma única estos sufijos entre bosques.  
  
-   Cuando un servidor de entidad de certificación (CA) de certificación de miembro de dominio cambia su nombre de dominio completo (FQDN) para que ya no use el mismo sufijo DNS principal que usa los controladores de dominio del dominio al que el servidor de CA es un miembro. En este caso, puede tener problemas de validación de certificados del servidor de CA emitido, dependiendo de qué nombres DNS se usan en los puntos de distribución de CRL. Pero si coloca un servidor de CA en un espacio de nombres no contiguo estable, funciona correctamente y es compatible.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Consideraciones para los espacios de nombres  
Las consideraciones siguientes pueden ayudarle a decidir si debe utilizar un espacio de nombres no contiguo.  
  
### <a name="application-compatibility"></a>Compatibilidad de aplicaciones  
Como se mencionó anteriormente, un espacio de nombres no contiguo puede causar problemas para todas las aplicaciones y servicios que se escriben en suponer que es idéntico al nombre del nombre de dominio del que es miembro un sufijo DNS principal de equipo. Antes de implementar un espacio de nombres separado, debe comprobar los problemas de compatibilidad de las aplicaciones. Además, asegúrese de comprobar la compatibilidad de todas las aplicaciones que usan al realizar el análisis. Esto incluye aplicaciones de Microsoft y de otros desarrolladores de software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Ventajas de los espacios de nombres  
Uso de un espacio de nombres no contiguo puede tener las siguientes ventajas:  
  
-   Dado que el sufijo DNS principal de un equipo puede indicar una información diferente, puede administrar el espacio de nombres DNS del nombre de dominio de Active Directory.  
  
-   Puede separar el espacio de nombres DNS en función de la ubicación geográfica o la estructura de la empresa. Por ejemplo, puede separar el espacio de nombres basado en los nombres de unidad de negocio o ubicación física como continente, país o región o creación.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Desventajas de los espacios de nombres  
Uso de un espacio de nombres no contiguo puede tener las siguientes desventajas:  
  
-   Debe crear y administrar zonas DNS independientes para cada dominio de Active Directory en el bosque que tenga equipos miembro que usan un espacio de nombres no contiguo. (Es decir, requiere una configuración adicional y más compleja.)  
  
-   Debe realizar pasos manuales para modificar y administrar el atributo de Active Directory que permite a los miembros de dominio utilizar los sufijos DNS primarios, especificados.  
  
-   Para optimizar la resolución de nombres, debe realizar pasos manuales para modificar y mantener la directiva de grupo para configurar los equipos miembros con alternativas sufijos DNS primarios.  
  
    > [!NOTE]  
    > El servicio de nombres Internet de Windows (WINS) se podría usar para desplazar esta desventaja mediante la resolución de nombres de etiqueta única. Para obtener más información acerca de WINS, consulte la referencia técnica de WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Cuando su entorno requiere varios sufijos DNS primarios, debe configurar correctamente el orden de búsqueda de sufijos DNS para todos los dominios de Active Directory del bosque.  
  
    Para establecer el orden de búsqueda de sufijos DNS, puede utilizar objetos de directiva de grupo o los parámetros de servicio del servidor de protocolo de configuración dinámica de Host (DHCP). También puede modificar el registro.  
  
-   Debe probar con cuidado todas las aplicaciones para los problemas de compatibilidad.  
  
Para obtener más información acerca de los pasos que puede tomar para abordar estos inconvenientes, consulte Crear un Namespace no contiguo ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Planear la transición de un espacio de nombres  
Antes de modificar un espacio de nombres, revise las consideraciones siguientes, que se aplican a las transiciones de los espacios de nombres contiguo discontinuas espacios de nombres (o viceversa):  
  
-   Configurar manualmente los nombres de entidad de servicio (SPN) puede dejarán de coincidir con los nombres DNS después de un cambio de espacio de nombres. Esto puede provocar errores de autenticación.  
  
    Para obtener más información, consulte servicio inicios de sesión de un error debido a Incorrectly Set SPNs ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Si utiliza equipos basados en Windows Server 2003 con la delegación restringida, esos equipos pueden requerir una configuración adicional para cambiar los SPN. Para obtener más información, consulte el artículo 936628 en Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Si desea delegar permisos para modificar los SPN subordinadas administradores, vea delegar la autoridad para modificar los SPN ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Si utiliza Protocolo ligero de acceso a directorios (LDAP) a través de Secure Sockets Layer (SSL) (conocido como LDAPS) con una entidad de certificación en una implementación que tiene controladores de dominio que se configuran en un espacio de nombres separado, debe usar el nombre de dominio de Active Directory adecuado y sufijo DNS principal al configurar los certificados LDAPS.  
  
    Para obtener más información sobre los requisitos de certificado de controlador de dominio, consulte el artículo 321051 de Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > Los controladores de dominio que usan certificados para LDAPS pueden requerir volver a implementar sus certificados. Al hacerlo, los controladores de dominio no pueden seleccionar un certificado adecuado, hasta que se reinicien. Para obtener más información sobre la autenticación de LDAPS y una actualización relacionada para Windows Server 2003, consulte el artículo 932834 en Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Planeamiento de implementaciones de espacio de nombres separado  
Si implementa los equipos en un entorno que tiene un espacio de nombres no contiguo, tomar las siguientes precauciones:  
  
1.  Notificar a todos los proveedores de software con el que hacer negocios que debe probar y admite un espacio de nombres no contiguo. Pídale que compruebe que admiten sus aplicaciones en entornos que usan los espacios de nombres.  
  
2.  Probar todas las versiones de sistemas operativos y aplicaciones en entornos de laboratorio de espacio de nombres separado. Al hacerlo, siga estas recomendaciones:  
  
    1.  Resuelva todos los problemas de software antes de implementar el software en su entorno.  
  
    2.  Cuando sea posible, participar en las pruebas beta de los sistemas operativos y aplicaciones que va a implementar en los espacios de nombres.  
  
3.  Asegúrese de que los administradores y personal de soporte técnico tener en cuenta el espacio de nombres separado y su impacto.  
  
4.  Crear un plan que hace posible para realizar la transición de un espacio de nombres separado a un espacio de nombres contiguo, si es necesario.  
  
5.  Dé a conocer la importancia de la compatibilidad de espacio de nombres separado con el sistema operativo y los proveedores de aplicaciones.  
  


