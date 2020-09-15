---
title: Consideraciones de LDAP en agrega optimización del rendimiento
description: Consideraciones de LDAP en cargas de trabajo de Active Directory
ms.topic: article
ms.author: timwi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: dec345b872b6a87d7d0a1414aef6fe9b6da39cc5
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077364"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Consideraciones de LDAP en agrega optimización del rendimiento

> [!IMPORTANT]
> A continuación se ofrece un resumen de las recomendaciones y consideraciones clave para optimizar el hardware de servidor para Active Directory cargas de trabajo que se tratan con mayor profundidad en el artículo [planeación de la capacidad de Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) . Es muy recomendable que los lectores revisen el [planeamiento de la capacidad de Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) para obtener una mayor comprensión técnica e implicaciones de estas recomendaciones.

## <a name="verify-ldap-queries"></a>Comprobar consultas LDAP

Compruebe que las consultas LDAP se ajustan a las recomendaciones para crear consultas eficaces.

Hay una amplia documentación sobre MSDN sobre cómo escribir, estructurar y analizar correctamente consultas para usarlas en Active Directory. Para obtener más información, vea [creación de aplicaciones más eficaces habilitadas para Microsoft Active Directory](/previous-versions/ms808539(v=msdn.10)).

## <a name="optimize-ldap-page-sizes"></a>Optimizar tamaños de página LDAP

Cuando se devuelven resultados con varios objetos en respuesta a solicitudes de cliente, el controlador de dominio tiene que almacenar temporalmente el conjunto de resultados en la memoria. Aumentar el tamaño de las páginas producirá más uso de memoria y puede reducir los elementos de la memoria caché innecesariamente. En este caso, la configuración predeterminada es óptima. Hay varios escenarios en los que se han realizado recomendaciones para aumentar la configuración del tamaño de página. Se recomienda usar los valores predeterminados a menos que se identifiquen específicamente como inadecuados.

Cuando las consultas tienen muchos resultados, es posible que se encuentre un límite de consultas similares que se ejecutan simultáneamente.  Esto se produce porque el servidor LDAP puede agotar un área de memoria global conocida como grupo de cookies.  Puede que sea necesario aumentar el tamaño del grupo tal y como se describe en [cómo se controlan las cookies del servidor LDAP](../../../../identity/ad-ds/manage/how-ldap-server-cookies-are-handled.md).

Para optimizar esta configuración, consulte [Windows Server 2008 y el controlador de dominio más reciente solo devuelve los valores 5000 en una respuesta LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Determinar si se van a agregar índices

Los atributos de indización son útiles al buscar objetos que tienen el nombre de atributo en un filtro. La indización puede reducir el número de objetos que se deben visitar al evaluar el filtro. Sin embargo, esto reduce el rendimiento de las operaciones de escritura porque el índice debe actualizarse cuando se modifica o se agrega el atributo correspondiente. También aumenta el tamaño de la base de datos de directorio, aunque las ventajas a menudo superan el costo del almacenamiento. El registro se puede usar para buscar consultas costosas e ineficaces. Una vez identificados, considere la posibilidad de indizar algunos atributos que se usan en las consultas correspondientes para mejorar el rendimiento de la búsqueda. Para obtener más información sobre cómo funcionan las búsquedas de Active Directory, vea [Cómo funcionan las búsquedas en Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc755809(v=ws.10)).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Escenarios que se benefician al agregar índices

-   La carga de cliente al solicitar los datos está generando un uso de CPU significativo y el comportamiento de las consultas de cliente no se puede cambiar ni optimizar. Por carga significativa, tenga en cuenta que se muestra en una lista de los 10 principales infractores en el asesor de rendimiento del servidor o en el conjunto de recopiladores de datos Active Directory integrados y usa más del 1% de la CPU.

-   La carga de cliente genera una e/s de disco significativa en un servidor debido a un atributo no indexado y el comportamiento de la consulta del cliente no se puede cambiar ni optimizar.

-   Una consulta está tardando mucho y no se completa en un período de tiempo aceptable para el cliente debido a la falta de índices de cobertura.

- Los grandes volúmenes de consultas con duraciones altas provocan el consumo y el agotamiento de los subprocesos LDAP de ATQ. Supervise los siguientes contadores de rendimiento:

    - **NTDS \\ Latencia de solicitudes** : esto está sujeto a cuánto tiempo tarda la solicitud en procesarse. Active Directory agota el tiempo de espera de las solicitudes después de 120 segundos (valor predeterminado), sin embargo, la mayoría debe ejecutarse mucho más rápido y las consultas de ejecución extremadamente larga deben ocultarse en los números generales. Busque los cambios en esta línea de base, en lugar de los umbrales absolutos.

        > [!NOTE]
        > Los valores altos aquí también pueden ser indicadores de retrasos en las solicitudes de "proxy" a otras comprobaciones de CRL y dominios.

    - **NTDS \\ Retraso en la cola Estimado** : Idealmente, esto debería estar cerca de 0 para un rendimiento óptimo, ya que esto significa que las solicitudes no pasan ningún tiempo en espera de servicio.

Estos escenarios se pueden detectar mediante uno o varios de los métodos siguientes:

-   [Determinar el tiempo de consulta con el control de estadísticas](/previous-versions/ms808539(v=msdn.10))

-   [Seguimiento de búsquedas costosas e ineficaces](/previous-versions/ms808539(v=msdn.10))

-   Active Directory conjunto de recopiladores de datos de diagnóstico en el monitor de rendimiento ([hijo de spa: conjuntos de recopiladores de datos de AD en Win2008 y versiones posteriores](/archive/blogs/askds/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond))

-   [Asesor de rendimiento de servidor de Microsoft](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Paquete de Active Directory Advisor

-   Realiza búsquedas con cualquier filtro además de "(objectClass = \* )" que usa el índice de antecesores.

### <a name="other-index-considerations"></a>Otras consideraciones sobre los índices

-   Asegúrese de que la creación del índice es la solución correcta para el problema después de que el ajuste de la consulta se haya agotado como una opción. Ajustar el tamaño del hardware correctamente es muy importante. Los índices solo deben agregarse si la corrección correcta es indexar el atributo y no se trata de un intento de ofuscar problemas de hardware.

-   Los índices aumentan el tamaño de la base de datos en un mínimo del tamaño total del atributo que se está indizando. Por consiguiente, una estimación del crecimiento de la base de datos se puede evaluar tomando el tamaño medio de los datos en el atributo y multiplicando por el número de objetos que tendrán el atributo rellenado. Por lo general, se trata de un aumento del 1% del tamaño de la base de datos. Para obtener más información, vea [Cómo funciona el almacén de datos](/previous-versions/windows/it-pro/windows-server-2003/cc772829(v=ws.10)).

-   Si el comportamiento de búsqueda se realiza principalmente en el nivel de la unidad de la organización, considere la posibilidad de indexar búsquedas en contenedor.

-   Los índices de tupla son más grandes que los índices normales, pero es mucho más difícil calcular el tamaño. Use cálculos de tamaño de índices normales como el piso para el crecimiento, con un máximo de un 20%. Para obtener más información, vea [Cómo funciona el almacén de datos](/previous-versions/windows/it-pro/windows-server-2003/cc772829(v=ws.10)).

-   Si el comportamiento de búsqueda se realiza principalmente en el nivel de la unidad de la organización, considere la posibilidad de indexar búsquedas en contenedor.

-   Los índices de tupla son necesarios para admitir cadenas de búsqueda medial y cadenas de búsqueda final. Los índices de tupla no son necesarios para las cadenas de búsqueda iniciales.

    -   Cadena de búsqueda inicial: (samAccountName = mi \* )

    -   Cadena de búsqueda medial: (samAccountName = \* mi \* )

    -   Cadena de búsqueda final: (samAccountName = \* mi $)

-   Al crear un índice se generará la e/s de disco mientras se compila el índice. Esto se hace en un subproceso en segundo plano con una prioridad más baja y las solicitudes entrantes se priorizarán en la compilación del índice. Si el planeamiento de la capacidad para el entorno se ha realizado correctamente, debería ser transparente. Sin embargo, los escenarios con mucha actividad de escritura o un entorno en el que se desconoce la carga en el almacenamiento del controlador de dominio podrían degradar la experiencia del cliente y deben deshacerse fuera del horario de trabajo.

-   El impacto en el tráfico de replicación es mínimo, ya que la creación de índices se produce de forma local.

Para obtener más información, vea lo siguiente:

-   [Creación de aplicaciones más eficaces habilitadas para Microsoft Active Directory](/previous-versions/ms808539(v=msdn.10))

-   [Búsqueda en Active Directory Domain Services](/windows/win32/ad/searching-in-active-directory-domain-services)

-   [Atributos indizados](/windows/win32/ad/indexed-attributes)

## <a name="additional-references"></a>Referencias adicionales

- [Optimización del rendimiento para servidores de Active Directory](index.md)
- [Consideraciones de hardware](hardware-considerations.md)
- [Colocación adecuada de los controladores de dominio y consideraciones de sitio](site-definition-considerations.md)
- [Solución de problemas de rendimiento de AD DS](troubleshoot.md)
- [Capacity Planning for Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) (Planeamiento de la capacidad para Active Directory Domain Services)