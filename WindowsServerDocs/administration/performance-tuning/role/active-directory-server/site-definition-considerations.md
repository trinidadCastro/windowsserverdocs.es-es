---
title: La ubicación del controlador de dominio y la definición del sitio en agrega optimización del rendimiento
description: Consideraciones sobre la ubicación del controlador de dominio y la definición del sitio en Active Directory optimización del rendimiento.
ms.topic: article
ms.author: timwi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fa051bd6637eff9f5a25cd8784d33a60095ccd54
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077202"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Colocación adecuada de los controladores de dominio y consideraciones de sitio

La definición adecuada del sitio es fundamental para el rendimiento. Los clientes que salen del sitio pueden experimentar un rendimiento deficiente de las autenticaciones y las consultas. Además, con la introducción de IPv6 en los clientes, la solicitud puede proviene de IPv4 o de la dirección IPv6 y Active Directory necesita tener sitios definidos correctamente para IPv6. El sistema operativo prefiere IPv6 a IPv4 cuando ambos están configurados.

A partir de Windows Server 2008, el controlador de dominio intenta usar la resolución de nombres para realizar una búsqueda inversa con el fin de determinar el sitio en el que debe estar el cliente. Esto puede provocar el agotamiento del grupo de subprocesos ATQ y hacer que el controlador de dominio deje de responder. La solución adecuada a esto es definir correctamente la topología del sitio para IPv6. Como alternativa, se puede optimizar la infraestructura de resolución de nombres para responder rápidamente a las solicitudes del controlador de dominio. Para obtener más información, consulte [respuesta diferida del controlador de dominio de Windows server 2008 o Windows server 2008 R2 a las solicitudes LDAP o Kerberos](https://support.microsoft.com/kb/2668820).

Un área de consideración adicional es la ubicación de los DC de lectura/escritura para los escenarios en los que se usan los RODC.  Ciertas operaciones requieren acceso a un controlador de dominio de escritura o destino a un controlador de dominio de escritura cuando un controlador de dominio de solo lectura sería suficiente.  La optimización de estos escenarios tomaría dos rutas de acceso:
-   Ponerse en contacto con controladores de dominio de escritura cuando un controlador de dominio de solo lectura sería suficiente.  Esto requiere un cambio en el código de la aplicación.
-   Dónde puede ser necesario un controlador de dominio grabable.  Coloque controladores de dominio de lectura y escritura en ubicaciones centrales para reducir la latencia.

Para obtener más información, consulte:
-   [Compatibilidad de aplicaciones con RODC](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772597(v=ws.10))
-   [Interfaz de servicio de Active Directory (ADSI) y controlador de dominio de solo lectura (RODC): evitar problemas de rendimiento](/archive/blogs/fieldcoding/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues)

## <a name="optimize-for-referrals"></a>Optimizar para referencias

Las referencias son el modo en que se redirigen las consultas LDAP cuando el controlador de dominio no hospeda una copia de la partición consultada. Cuando se devuelve una referencia, contiene el nombre distintivo de la partición, un nombre DNS y un número de puerto. El cliente usa esta información para continuar con la consulta en un servidor que hospeda la partición. Se trata de un escenario ubicador y se conservan todas las definiciones de sitios de recomendaciones y la ubicación del controlador de dominio, pero las aplicaciones que dependen de las referencias se suelen pasar por alto. Se recomienda asegurarse de que la topología de AD, incluidas las definiciones de sitio y la ubicación del controlador de dominio, refleje adecuadamente las necesidades del cliente. Además, esto puede incluir tener controladores de dominio de varios dominios en un solo sitio, optimizar la configuración de DNS o cambiar el sitio de una aplicación.

## <a name="optimization-considerations-for-trusts"></a>Consideraciones de optimización para confianzas

En un escenario dentro de un bosque, las confianzas se procesan de acuerdo con la siguiente jerarquía de dominio: dominio secundario de los dominios secundarios-secundarios dominio raíz del bosque-dominio &gt; &gt; &gt; secundario-secundario &gt; . Esto significa que los canales seguros en la raíz del bosque y cada elemento primario se pueden sobrecargar debido a la agregación de solicitudes de autenticación que transitan por los controladores de seguridad de la jerarquía de confianza. Esto también puede provocar retrasos en Active Directory de grandes dispersaciones geográficas cuando la autenticación también tiene que pasar vínculos de alta latencia para afectar al flujo anterior. Las sobrecargas pueden producirse en escenarios de confianza de nivel inferior y entre bosques. Las siguientes recomendaciones se aplican a todos los escenarios:

-   Ajuste correctamente el MaxConcurrentAPI para admitir la carga a través del canal seguro. Para obtener más información, vea [Cómo realizar el ajuste del rendimiento para la autenticación NTLM mediante la configuración MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Cree confianzas de acceso directo según corresponda en función de la carga.

-   Asegúrese de que todos los controladores de dominio del dominio pueden realizar la resolución de nombres y comunicarse con los controladores de dominio en el dominio de confianza.

-   Asegúrese de que se tienen en cuenta las consideraciones de localidad para las confianzas.

-   Habilite Kerberos siempre que sea posible y minimice el uso del canal seguro para reducir el riesgo de que se produzcan cuellos de botella de MaxConcurrentAPI.

Los escenarios de confianza entre dominios son un área que ha sido sistemáticamente un punto problemático para muchos clientes. Los problemas de resolución de nombres y conectividad, a menudo debido a los firewalls, causan el agotamiento de los recursos en el controlador de dominio que confía y afectan a todos los clientes. Además, un escenario que a menudo se pasa por alto es optimizar el acceso a los controladores de dominio de confianza. Las áreas clave para asegurarse de que funciona correctamente son las siguientes:

-   Asegúrese de que la resolución de nombres DNS y WINS que usan los controladores de dominio que confían puede resolver una lista precisa de controladores de dominio para el dominio de confianza.

    -   Los registros agregados de forma estática tienen tendencia a ser obsoletos y volver a introducir los problemas de conectividad con el tiempo. Los reenvíos de DNS, DNS dinámicos y la combinación de infraestructuras WINS/DNS son más mantenibles en la larga ejecución.

    -   Asegúrese de configurar correctamente los reenviadores, los reenvíos condicionales y las copias secundarias para las zonas de búsqueda directa e inversa para cada recurso del entorno al que un cliente pueda tener acceso. De nuevo, esto requiere el mantenimiento manual y tiene una tendencia a quedar obsoleta. La consolidación de infraestructuras es ideal.

-   Los controladores de dominio del dominio que confía intentarán localizar los controladores de dominio en el dominio de confianza que se encuentran en el mismo sitio y, a continuación, realizar la conmutación por recuperación a los localizadores genéricos.

    -   Para obtener más información sobre cómo funciona ubicador, consulte [Buscar un controlador de dominio en el sitio más cercano](/previous-versions/windows/it-pro/windows-2000-server/cc978016(v=technet.10)).

    -   Converge los nombres de sitio entre los dominios de confianza y de confianza para reflejar el controlador de dominio en la misma ubicación. Asegúrese de que las asignaciones de subred y dirección IP estén vinculadas correctamente a sitios de ambos bosques. Para obtener más información, consulte [localizador de dominios a través de una confianza de bosque](/archive/blogs/askds/domain-locator-across-a-forest-trust).

    -   Asegúrese de que los puertos están abiertos, según las necesidades de ubicador, para la ubicación del controlador de dominio. Si existen firewalls entre los dominios, asegúrese de que los firewalls estén configurados correctamente para todas las confianzas. Si los firewalls no están abiertos, el controlador de dominio que confía seguirá intentando tener acceso al dominio de confianza. Si se produce un error en la comunicación por cualquier motivo, el controlador de dominio que confía finalmente agotará el tiempo de espera de la solicitud al controlador de dominio de confianza. Sin embargo, estos tiempos de espera pueden tardar varios segundos por solicitud y pueden agotar los puertos de red en el controlador de dominio que confía si el volumen de solicitudes entrantes es elevado. El cliente puede experimentar el tiempo de espera para el controlador de dominio como subprocesos bloqueados, que podrían traducirse en aplicaciones bloqueadas (si la aplicación ejecuta la solicitud en el subproceso en primer plano). Para obtener más información, consulte [configuración de un firewall para dominios y confianzas](https://support.microsoft.com/kb/179442).

    -   Use DnsAvoidRegisterRecords para eliminar los controladores de dominio con un rendimiento deficiente o una latencia alta, como los de los sitios satélite, de la publicidad a los localizadores genéricos. Para obtener más información, consulte [optimización de la ubicación de un controlador de dominio o catálogo global que reside fuera del sitio de un cliente](https://support.microsoft.com/kb/306602).

        > [!NOTE]
        > Hay un límite práctico de aproximadamente 50 al número de controladores de dominio que el cliente puede consumir. Estos deben ser los controladores de dominio de mayor capacidad y óptimas para el sitio.


    -  Considere la posibilidad de colocar controladores de dominio de dominios de confianza y de confianza en la misma ubicación física.

En todos los escenarios de confianza, las credenciales se enrutan según el dominio especificado en las solicitudes de autenticación. Esto también se aplica a las consultas a LookupAccountName y LsaLookupNames (así como a otras que son solo las que se usan con más frecuencia). Cuando se pasa un valor NULL a los parámetros de dominio para estas API, el controlador de dominio intentará encontrar el nombre de cuenta especificado en todos los dominios de confianza disponibles.

-   Deshabilitar la comprobación de todas las confianzas disponibles cuando se especifica un dominio nulo. [Restricción de la búsqueda de nombres aislados en dominios de confianza externos mediante la entrada del registro LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Deshabilitar el paso de solicitudes de autenticación con un dominio nulo especificado en todas las confianzas disponibles. [El proceso de Lsass.exe puede dejar de responder si tiene muchas confianzas externas en un controlador de dominio Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="additional-references"></a>Referencias adicionales
- [Optimización del rendimiento para servidores de Active Directory](index.md)
- [Consideraciones de hardware](hardware-considerations.md)
- [Consideraciones de LDAP](ldap-considerations.md)
- [Solución de problemas de rendimiento de AD DS](troubleshoot.md)
- [Capacity Planning for Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) (Planeamiento de la capacidad para Active Directory Domain Services)