---
title: Ubicación del controlador de dominio y de definición de sitio en ADDS optimización del rendimiento
description: Sitio definición y el dominio controlador colocación consideraciones en la optimización del rendimiento de Active Directory.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0e5c5f8b7cf5c028fbfa5d72c4bc1218565d4087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814026"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Colocación adecuada de los controladores de dominio y consideraciones de sitio

Definición de sitio adecuada es vital para el rendimiento. Los clientes fuera del sitio pueden experimentar un rendimiento deficiente de las autenticaciones y consultas. Además, con la introducción de IPv6 en los clientes, la solicitud puede provenir de IPv4 o la dirección IPv6 y Active Directory debe tener sitios correctamente definidos para IPv6. El sistema operativo prefiere IPv6 a IPv4 cuando ambas están configuradas.

A partir de Windows Server 2008, los intentos de controlador de dominio a usar la resolución de nombres para realizar una búsqueda inversa con el fin de determinar el sitio del cliente deben estar en. Esto puede provocar el agotamiento del grupo de subprocesos ATQ y hacer que el controlador de dominio deje de responder. La resolución adecuada para esto es definir correctamente la topología de sitios para IPv6. Como alternativa, uno puede optimizar la infraestructura de resolución de nombres para responder rápidamente a las solicitudes del controlador de dominio. Para obtener más información, consulte [Windows Server 2008 o el controlador de dominio de Windows Server 2008 R2 retrasa la respuesta a las solicitudes LDAP o Kerberos](https://support.microsoft.com/kb/2668820).

Un área de consideración adicional es buscar controladores de dominio de lectura/escritura para escenarios donde los RODC están en uso.  Ciertas operaciones requieren acceso a un controlador de dominio de escritura o un controlador de dominio de escritura de destino cuando bastaría con un controlador de dominio de solo lectura.  Optimización de estos escenarios, tendría dos rutas de acceso:
-   Ponerse en contacto con los controladores de dominio grabable cuando bastaría con un controlador de dominio de solo lectura.  Esto requiere un cambio de código de aplicación.
-   En un controlador de dominio de escritura puede ser necesario.  Controladores de dominio de lectura y escritura lugar en ubicaciones centrales para minimizar la latencia.

Como referencia de información adicional:
-   [Compatibilidad de aplicaciones con los RODC](https://technet.microsoft.com/library/cc772597.aspx)
-   [Interfaz de servicio de Active Directory (ADSI) y la lectura solo controlador de dominio (RODC): evitar problemas de rendimiento](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Optimizar para las referencias

Las referencias son cómo se redirigen las consultas LDAP cuando el controlador de dominio no hospeda una copia de la partición de consulta. Cuando se devuelve una referencia, que contiene el nombre distintivo de la partición, un nombre DNS y un número de puerto. El cliente usa esta información para continuar con la consulta en un servidor que hospeda la partición. Este es un escenario del ubicador de DC y todas las recomendaciones las definiciones de sitios y se mantiene la ubicación del controlador de dominio, pero las aplicaciones que dependen de las referencias a menudo se pasa por alto. Se recomienda para garantizar la topología de AD, incluidas las definiciones de sitio y la ubicación del controlador de dominio refleja adecuadamente las necesidades del cliente. Además, esto puede incluir la necesidad de controladores de dominio en varios dominios en un único sitio, ajuste la configuración de DNS, o cambiar la ubicación del sitio de una aplicación.

## <a name="optimization-considerations-for-trusts"></a>Consideraciones sobre la optimización de confianzas

En un escenario dentro del bosque, las confianzas se procesan según la jerarquía de dominios siguientes: Dominio secundarios -&gt; dominio secundario -&gt; del bosque de dominio raíz -&gt; dominio secundario -&gt; dominios secundarios. Esto significa que protegen los canales a la raíz del bosque y cada elemento primario, puede sobrecargarse debido a la agregación de las solicitudes de autenticación a los controladores de dominio en la jerarquía de confianza en tránsito. También esto puede dar lugar a retrasos en directorios de Active Directory de gran tamaño dispersión geográfica cuando la autenticación también tiene que tienen una latencia elevada vínculos para afectar al flujo anterior de tránsito. Las sobrecargas pueden producirse en escenarios de confianza entre bosques y de nivel inferior. Las siguientes recomendaciones se aplican a todos los escenarios:

-   Ajustar correctamente el MaxConcurrentAPI para admitir la carga en el canal seguro. Para obtener más información, consulte [cómo optimizar el rendimiento de la autenticación NTLM mediante el uso de la configuración de MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Crear confianzas según corresponda en función de carga.

-   Asegúrese de que cada controlador de dominio en el dominio es capaz de realizar la resolución y comunicarse con los controladores de dominio del dominio de confianza.

-   Asegúrese de que las consideraciones de localidad se tienen en cuenta para las confianzas.

-   Habilitar Kerberos siempre que sea posible y minimizar el uso del canal seguro para reducir el riesgo de que se produzcan cuellos de botella de MaxConcurrentAPI.

Confianza entre dominios escenarios son un área que ha sido sistemáticamente una dificultad para muchos clientes. Problemas de conectividad y la resolución de nombres, a menudo debido a los firewalls, provocar el agotamiento de recursos en el controlador de dominio que confía y afectan a todos los clientes. Además, un escenario muy a menudo soslayado optimiza el acceso a los controladores de dominio de confianza. Las áreas clave para asegurarse de que funciona correctamente son los siguientes:

-   Asegúrese de que la resolución de nombres DNS y WINS que usan los controladores de dominio que confía puede resolver una lista precisa de los controladores de dominio del dominio de confianza.

    -   Se agregó estáticamente registros tienen tendencia a se conviertan en obsoletos y volver a introducir problemas de conectividad con el tiempo. DNS reenvía, DNS dinámico, y combinación de infraestructuras de DNS o WINS son más fáciles de mantener a largo plazo.

    -   Garantizar la correcta configuración de reenviadores, reenvía condicional y las copias secundarias para ambas zonas de búsqueda directa e inversa para todos los recursos en el entorno que un cliente puede necesitar acceso. Nuevamente, esto requiere el mantenimiento manual y tiene una tendencia se vuelvan obsoletos. La consolidación de infraestructuras es ideal.

-   Los controladores de dominio del dominio que confía intentará localizar controladores de dominio en el dominio de confianza que se encuentran en el mismo sitio en primer lugar y, a continuación, la conmutación por recuperación a los localizadores genéricos.

    -   Para obtener más información sobre el funcionamiento del ubicador de DC, consulte [encontrar un controlador de dominio en el sitio más cercano](https://technet.microsoft.com/library/cc978016.aspx).

    -   Converger los nombres de sitio entre los dominios que confían y de confianza para reflejar el controlador de dominio en la misma ubicación. Asegúrese de subred y la dirección IP asignaciones están vinculadas correctamente a sitios de ambos bosques. Para obtener más información, consulte [localizador a través de un bosque de confianza de dominio](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Asegúrese de que los puertos están abiertos, según las necesidades del ubicador de DC, para la ubicación del controlador de dominio. Si existen los firewalls entre los dominios, asegúrese de que los firewalls están configurados correctamente para todas las confianzas. Si los servidores de seguridad no están abiertos, el controlador de dominio que confía seguirá intentando acceder al dominio de confianza. Si se produce un error en la comunicación por cualquier motivo, el controlador de dominio que confía finalmente agotará el tiempo de la solicitud al controlador de dominio de confianza. Sin embargo, estos tiempos de espera puede tardar varios segundos por solicitud y puede agotar los puertos de red en el controlador de dominio que confía si el volumen de solicitudes entrantes es alta. El cliente puede experimentar la espera hasta el tiempo de espera en el controlador de dominio como subprocesos bloqueados, lo que podrían traducirse en aplicaciones bloqueadas (si la aplicación ejecuta la solicitud en el subproceso en primer plano). Para obtener más información, consulte [cómo configurar un firewall para dominios y confianzas de](https://support.microsoft.com/kb/179442).

    -   Utilice DnsAvoidRegisterRecords para eliminar controladores de dominio mal rendimiento o alta latencia, como los de los sitios de satélite, con la publicidad a los localizadores genéricos. Para obtener más información, consulte [cómo optimizar la ubicación de un controlador de dominio o catálogo global que reside fuera de un sitio del cliente](https://support.microsoft.com/kb/306602).

        **Tenga en cuenta**    hay un límite práctico de aproximadamente 50 y el número de controladores de dominio puede consumir el cliente. Mayor capacidad óptima para el sitio y más alta deben ser controladores de dominio.

         

    -   Considere la posibilidad de colocar los controladores de dominio de dominios de confianza y que confían en la misma ubicación física.

Todos los escenarios de confianza, las credenciales se enrutan según el dominio especificado en las solicitudes de autenticación. Esto también es cierto para las consultas al LookupAccountName y LsaLookupNames (así como otros usuarios, estos simplemente el más frecuente) API. Cuando los parámetros de dominio para estas API se pasa un valor NULL, el controlador de dominio intentará encontrar el nombre de cuenta especificado en cada dominio de confianza disponible.

-   Deshabilitar la comprobación de todas las confianzas disponibles cuando se especifica el dominio es NULL. [Cómo restringir la búsqueda de nombres aislados en dominios de confianza externos mediante el uso de la entrada de Registro LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Deshabilitar pasa las solicitudes de autenticación con el dominio NULL especificada en todas las confianzas disponibles. [El proceso de Lsass.exe deja de responder si dispone de muchas de las confianzas externas en un controlador de dominio de Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Vea también
- [Servidores de Active Directory de optimización del rendimiento](index.md)
- [Consideraciones de hardware](hardware-considerations.md)
- [Consideraciones de LDAP](ldap-considerations.md)
- [Solución de problemas de rendimiento de ADDS](troubleshoot.md) 
- [Planear la capacidad de servicios de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)