---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Espacio de nombres separado
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 06506ac47fe22724047cff73fb88972b868600b5
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939285"
---
# <a name="disjoint-namespace"></a>Espacio de nombres separado

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un espacio de nombres separado se produce cuando uno o más equipos miembros del dominio tienen un sufijo del servicio de nombres de dominio (DNS) principal que no coincide con el nombre DNS del dominio Active Directory del que son miembros los equipos. Por ejemplo, un equipo miembro que usa un sufijo DNS principal de corp.fabrikam.com en un dominio de Active Directory denominado na.corp.fabrikam.com usa un espacio de nombres separado.

Un espacio de nombres discontinuo es más complejo de administrar, mantener y solucionar problemas que un espacio de nombres contiguo. En un espacio de nombres contiguo, el sufijo DNS principal coincide con el nombre de dominio Active Directory. Las aplicaciones de red que se escriben suponen que el espacio de nombres Active Directory es idéntico al sufijo DNS principal para todos los equipos miembros del dominio no funcionan correctamente en un espacio de nombres separado.

## <a name="support-for-disjoint-namespaces"></a>Compatibilidad con espacios de nombres separados

Los equipos miembros del dominio, incluidos los controladores de dominio, pueden funcionar en un espacio de nombres discontinuo. Los equipos miembros del dominio pueden registrar su registro de recursos de host (A) y el registro de recursos de host de IP versión 6 (IPv6) (AAAA) en un espacio de nombres DNS separado. Cuando los equipos miembros del dominio registran sus registros de recursos de esta manera, los controladores de dominio continúan registrando los registros de recursos globales y específicos del sitio (SRV) en la zona DNS que es idéntico al nombre de dominio de Active Directory.

Por ejemplo, supongamos que un controlador de dominio para el dominio Active Directory denominado na.corp.fabrikam.com que usa un sufijo DNS principal de corp.fabrikam.com registra registros de recursos de host (A) y host IPv6 (AAAA) en la zona DNS corp.fabrikam.com. El controlador de dominio sigue registrando registros de recursos globales y específicos del sitio (SRV) en las zonas DNS _msdcs. na. Corp. fabrikam. com y na.corp.fabrikam.com, lo que hace posible la ubicación del servicio.

> [!IMPORTANT]
> Aunque los sistemas operativos Windows pueden admitir un espacio de nombres separado, las aplicaciones que se escriben suponen que el sufijo DNS principal es el mismo que el sufijo de dominio de Active Directory puede que no funcione en este tipo de entorno. Por esta razón, debe probar detenidamente todas las aplicaciones y sus respectivos sistemas operativos antes de implementar un espacio de nombres separado.

Un espacio de nombres discontinuo debe funcionar (y se admite) en las situaciones siguientes:

- Cuando un bosque con varios dominios de Active Directory usa un solo espacio de nombres DNS, que también se conoce como zona DNS

    Un ejemplo de esto es una empresa que usa dominios regionales con nombres como na.corp.fabrikam.com, sa.corp.fabrikam.com y asia.corp.fabrikam.com y usa un solo espacio de nombres DNS, como corp.fabrikam.com.

- Cuando un solo dominio de Active Directory se divide en espacios de nombres DNS independientes

    Un ejemplo de esto es una empresa con un Active Directory dominio de corp.contoso.com que usa zonas DNS como hr.corp.contoso.com, production.corp.contoso.com y it.corp.contoso.com.

Un espacio de nombres separado no funciona correctamente (y no se admite) en las situaciones siguientes:

- Un sufijo separado utilizado por los miembros del dominio coincide con un nombre de dominio de Active Directory en este u otro bosque. Esto rompe el enrutamiento del sufijo de nombre Kerberos.

- Se utiliza el mismo sufijo separado en otro bosque. Esto evita que el enrutamiento de estos sufijos sea único entre bosques.

- Cuando un servidor de entidad de certificación (CA) de un miembro de dominio cambia su nombre de dominio completo (FQDN) para que ya no use el mismo sufijo DNS principal que usan los controladores de dominio del dominio al que pertenece el servidor de CA. En este caso, es posible que tenga problemas al validar los certificados emitidos por el servidor de CA, en función de los nombres DNS que se usen en los puntos de distribución de CRL. Pero si coloca un servidor de CA en un espacio de nombres no contiguo y estable, funciona correctamente y es compatible.

## <a name="considerations-for-disjoint-namespaces"></a>Consideraciones para espacios de nombres separados

Las consideraciones siguientes pueden ayudarle a decidir si debe usar un espacio de nombres separado.

### <a name="application-compatibility"></a>Compatibilidad de aplicación

Como se mencionó anteriormente, un espacio de nombres separado puede causar problemas para cualquier aplicación y servicio que se escriba para asumir que un sufijo DNS principal del equipo es idéntico al nombre del nombre de dominio del que es miembro. Antes de implementar un espacio de nombres separado, debe comprobar si hay problemas de compatibilidad en las aplicaciones. Además, asegúrese de comprobar la compatibilidad de todas las aplicaciones que usa al realizar el análisis. Esto incluye las aplicaciones de Microsoft y de otros desarrolladores de software.

### <a name="advantages-of-disjoint-namespaces"></a>Ventajas de los espacios de nombres separados

El uso de un espacio de nombres separado puede tener las siguientes ventajas:

- Dado que el sufijo DNS principal de un equipo puede indicar información diferente, puede administrar el espacio de nombres DNS por separado desde el Active Directory nombre de dominio.

- Puede separar el espacio de nombres DNS en función de la estructura de negocio o la ubicación geográfica. Por ejemplo, puede separar el espacio de nombres en función de los nombres de las unidades de negocio o de la ubicación física como el continente, el país o la región o la compilación.

### <a name="disadvantages-of-disjoint-namespaces"></a>Desventajas de los espacios de nombres separados

El uso de un espacio de nombres separado puede tener las siguientes desventajas:

- Debe crear y administrar zonas DNS independientes para cada dominio de Active Directory del bosque que tenga equipos miembro que utilicen un espacio de nombres discontinuo. (Es decir, requiere una configuración adicional y más compleja).

- Debe realizar pasos manuales para modificar y administrar el Active Directory atributo que permite a los miembros del dominio usar sufijos DNS primarios especificados.

- Para optimizar la resolución de nombres, debe realizar pasos manuales para modificar y mantener directiva de grupo para configurar los equipos miembros con sufijos DNS principales alternativos.

> [!NOTE]
> El servicio de nombres Internet de Windows (WINS) se puede usar para desplazar este inconveniente resolviendo los nombres de etiqueta única. Para obtener más información acerca de WINS, consulte la [referencia técnica de WINS](/previous-versions/windows/it-pro/windows-server-2003/cc736411(v=ws.10)).

- Cuando su entorno requiere varios sufijos DNS principales, debe configurar el orden de búsqueda de sufijos DNS para todos los dominios de Active Directory del bosque de forma adecuada.

    Para establecer el orden de búsqueda de sufijos DNS, puede usar directiva de grupo objetos o parámetros de servicio de servidor de protocolo de configuración dinámica de host (DHCP). También puede modificar el registro.

- Debe probar detenidamente todas las aplicaciones en busca de problemas de compatibilidad.

Para obtener más información sobre los pasos que puede seguir para solucionar estas desventajas, consulte [crear un espacio de nombres discontinuo](/previous-versions/windows/it-pro/windows-server-2003/cc755926(v=ws.10)).

### <a name="planning-a-namespace-transition"></a>Planear una transición de espacio de nombres

Antes de modificar un espacio de nombres, revise las consideraciones siguientes, que se aplican a las transiciones de espacios de nombres contiguos a espacios de nombres separados (o a la inversa):

- Los nombres de entidad de seguridad de servicio (SPN) configurados manualmente ya no pueden coincidir con los nombres DNS tras un cambio de espacio de nombres. Esto puede producir errores de autenticación.

    Para obtener más información, vea [errores de inicio de sesión de servicio debido a la configuración incorrecta de SPN](/previous-versions/windows/it-pro/windows-server-2003/cc772897(v=ws.10)).

    - Si usa equipos basados en Windows Server 2003 con delegación restringida, es posible que estos equipos requieran configuración adicional para cambiar los SPN. Para obtener más información, consulte el artículo 936628 en Microsoft Knowledge base, [el SPN no aparece en la lista de servicios que se pueden delegar a una cuenta cuando se intenta configurar la delegación restringida en un equipo que ejecuta Windows Server 2003](https://support.microsoft.com/help/936628) (404).

    - Si desea delegar permisos para modificar SPN en administradores subordinados, vea [delegar autoridad para modificar SPN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770439(v=ws.10)).

- Si usa el Protocolo ligero de acceso a directorios (LDAP) sobre Capa de sockets seguros (SSL) (conocido como LDAPs) con una CA en una implementación que tiene controladores de dominio que están configurados en un espacio de nombres separado, debe usar el nombre de dominio de Active Directory apropiado y el sufijo DNS principal al configurar los certificados LDAPs.

    Para obtener más información acerca de los requisitos de certificado de controlador de dominio, consulte el artículo 321051 en Microsoft Knowledge base, [How to habilitate LDAP over SSL with a terceros Certification Authority](https://support.microsoft.com/help/321051/).

    > [!NOTE]
    > Los controladores de dominio que usan certificados para LDAPs pueden requerir que vuelva a implementar sus certificados. Al hacerlo, es posible que los controladores de dominio no seleccionen un certificado adecuado hasta que se reinicien. Para obtener más información acerca de la autenticación LDAP (Protocolo ligero de acceso a directorios) sobre Capa de sockets seguros (SSL) (LDAPs) para Windows Server 2003, consulte el artículo 938703 de Microsoft Knowledge base, [How to TROUBLESHOOT LDAP on SSL Connection problems](https://support.microsoft.com/help/938703/).

### <a name="planning-for-disjoint-namespace-deployments"></a>Planeación de implementaciones de espacios de nombres disjuntos

Tenga en cuenta las siguientes precauciones si implementa equipos en un entorno que tiene un espacio de nombres separado:

1. Notifique a todos los proveedores de software con quienes usted hace negocios que deben probar y admiten un espacio de nombres separado. Pídales que comprueben que son compatibles con sus aplicaciones en entornos que usan espacios de nombres no contiguos.

2. Pruebe todas las versiones de sistemas operativos y aplicaciones en entornos de laboratorio de espacio de nombres no contiguos. Cuando lo haga, siga estas recomendaciones:

    1. Resuelva todos los problemas de software antes de implementar el software en su entorno.

    2. Cuando sea posible, participe en las pruebas beta de los sistemas operativos y las aplicaciones que planea implementar en espacios de nombres no contiguos.

3. Asegúrese de que los administradores y el personal del Departamento de soporte técnico conocen el espacio de nombres separado y su impacto.

4. Cree un plan que le permita realizar la transición de un espacio de nombres separado a un espacio de nombres contiguo, si es necesario.

5. Distribuya la importancia de la compatibilidad con espacios de nombres separados con el sistema operativo y los proveedores de aplicaciones.
