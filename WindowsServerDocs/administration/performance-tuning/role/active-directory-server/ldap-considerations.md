---
title: Consideraciones de LDAP en la optimización del rendimiento de ADDS
description: Consideraciones de LDAP en cargas de trabajo de Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 79f95c88c49d384f8a13b8808c63a0dc00de53cb
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266630"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Consideraciones de LDAP en la optimización del rendimiento de ADDS

>[!Important]
> El siguiente es un resumen de las recomendaciones más importantes y consideraciones para optimizar el hardware de servidor para las cargas de trabajo de Active Directory tratados en mayor profundidad en el [planear la capacidad de los servicios de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) artículo. Se recomienda encarecidamente no solo para revisar los lectores [planear la capacidad de los servicios de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) para un mayor conocimiento técnico y las implicaciones de estas recomendaciones.

## <a name="verify-ldap-queries"></a>Compruebe las consultas LDAP

Compruebe que las consultas LDAP cumplan con las recomendaciones de consultas eficaces de creación.

Hay una amplia documentación en MSDN sobre cómo escribir, estructura y analizar las consultas para su uso en Active Directory correctamente. Para obtener más información, consulte [Creating More Efficient Microsoft Active Directory-Enabled aplicaciones](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Optimizar los tamaños de página LDAP

Cuando se devuelven resultados con varios objetos en respuesta a las solicitudes de cliente, el controlador de dominio tiene que almacenar temporalmente el conjunto de resultados en memoria. Cada vez mayor de los tamaños de página hará que el mayor uso de memoria y pueden age innecesariamente los elementos de la caché. En este caso, la configuración predeterminada es óptimo. Hay varios escenarios donde se realizaron las recomendaciones para aumentar los valores de tamaño de página. Se recomienda usar los valores predeterminados a menos que se identifica específicamente como inadecuado.

Cuando las consultas tienen muchos resultados, se puede encontrar un límite de consultas similares que se ejecutan simultáneamente.  Esto se produce como un área de memoria global, conocido como el grupo de cookies que puede reducir el servidor LDAP.  Puede que sea necesario aumentar el tamaño del grupo como se describe en [cómo LDAP Server se controlan las Cookies](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Para ajustar esta configuración, consulte [controlador de dominio de 2008 y versiones más recientes de Windows Server devuelve sólo 5000 valores en una respuesta LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Determinar si se debe agregar índices

La indización de atributos es útil cuando se buscan los objetos que tienen el nombre del atributo en un filtro. La indización puede reducir el número de objetos que se debe visitar al evaluar el filtro. Sin embargo, esto reduce el rendimiento de las operaciones de escritura porque el índice debe actualizarse cuando se modifica o agrega el atributo correspondiente. También aumenta el tamaño de la base de datos de directorio, aunque a menudo, las ventajas superan el costo del almacenamiento. Registro se puede utilizar para buscar las consultas costosas e ineficaces. Una vez identificadas, considere la posibilidad de indizar algunos atributos que se usan en las consultas correspondientes para mejorar el rendimiento de la búsqueda. Para obtener más información acerca del funcionamiento de las búsquedas en Active Directory, consulte [Active Directory búsquedas funcionamiento](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Escenarios que se benefician de adición de índices

-   Carga del cliente en el que solicita los datos está generando uso significativo de CPU y el comportamiento de la consulta de cliente no se puede cambiar ni optimizado. Por carga significativa, considere la posibilidad de que se muestra en una lista infractor Top 10 Server Performance Advisor o el integrada Active Directory datos conjunto de recopiladores y está usando más de un 1% de CPU.

-   La carga del cliente es generar E/S de disco significativo en un servidor debido a un atributo no indizado y el comportamiento de la consulta de cliente no se puede cambiar ni optimizado.

-   Una consulta está tardando mucho tiempo y no se completa en un período de tiempo aceptable para el cliente debido a la falta de índices de cobertura.

-   Grandes volúmenes de consultas con duraciones alta causan agotamiento de subprocesos de ATQ LDAP y el consumo. Supervisar los contadores de rendimiento siguientes:

    -   **NTDS\\latencia de solicitud** : esto está sujeto a cuánto se tarda en procesar la solicitud. Active Directory agota el tiempo de las solicitudes después de 120 segundos (valor predeterminado), sin embargo, la mayoría se debe ejecutar con más rapidez y consultas de ejecución muy prolongada debería obtener ocultas en los números general. Busque los cambios de esta línea base, en lugar de los umbrales absolutos.

        > [!Note]   Los valores altos aquí también pueden ser indicadores de retrasos en "proxy" solicitudes a otros dominios y comprobaciones CRL.


    -   **NTDS\\estimado retardo de cola** : lo ideal es que debe ser cercanos a 0 para un rendimiento óptimo ya que esto significa que las solicitudes no pasan ningún tiempo de espera para ser atendidas.

Estos escenarios se pueden detectar mediante uno o varios de los métodos siguientes:

-   [Determinar el tiempo de una consulta con el Control de las estadísticas](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Seguimiento de las búsquedas costosas e ineficaces](https://msdn.microsoft.com/library/ms808539.aspx)

-   Conjunto de recopiladores de datos de diagnóstico de Active Directory en el Monitor de rendimiento ([hijo de SPA: En Win2008 y más allá de los conjuntos de recopiladores de datos de AD](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) módulo del Asesor de actualizaciones de Active Directory

-   Las búsquedas mediante cualquier filtro además "(objectClass =\*)" que usan el índice de antecesores.

### <a name="other-index-considerations"></a>Otras consideraciones de índice

-   Asegúrese de que crear el índice es la solución adecuada para el problema después de que se ha agotado la optimización de la consulta como una opción. Es muy importante cambiar correctamente el tamaño de hardware. Los índices se deben agregar solo cuando la solución correcta consiste en indexar el atributo y no intenta ofuscar los problemas de hardware.

-   Los índices de aumentar el tamaño de la base de datos en un mínimo del tamaño total del atributo que se está indizando. Una estimación de crecimiento de la base de datos, por tanto, se puede evaluar tomando el tamaño medio de los datos en el atributo y multiplicar por el número de objetos que tendrán el atributo de rellenado. Esto suele sobre un aumento de 1% de tamaño de base de datos. Para obtener más información, consulte [Data Store funcionamiento](https://technet.microsoft.com/library/cc772829.aspx).

-   Si el comportamiento de la búsqueda se realiza principalmente en el nivel de unidad de organización, considere la posibilidad de indizar para búsquedas de contenido.

-   Los índices de tupla son mayores que los índices normales, pero es mucho más difícil de estimar el tamaño. Usar índices normales calcula el tamaño como el límite inferior para el crecimiento, con un máximo de 20%. Para obtener más información, consulte [Data Store funcionamiento](https://technet.microsoft.com/library/cc772829.aspx).

-   Si el comportamiento de la búsqueda se realiza principalmente en el nivel de unidad de organización, considere la posibilidad de indizar para búsquedas de contenido.

-   Índices de tupla son necesarios para admitir las cadenas de búsqueda medio y cadenas de búsqueda final. Los índices de la tupla no son necesarios para las cadenas de búsqueda inicial.

    -   Inicial de la cadena de búsqueda: (samAccountName = Mi PC\*)

    -   Cadena de búsqueda medio - (samAccountName =\*Mi PC\*)

    -   Cadena de búsqueda final – (samAccountName =\*Mi PC$)

-   Creación de un índice generará la E/S de disco mientras se está compilando el índice. Esto se realiza en un subproceso en segundo plano con prioridad más baja y las solicitudes entrantes se priorizará sobre la generación del índice. Si se ha realizado correctamente el planeamiento de capacidad para el entorno, debe ser transparente. Sin embargo, los escenarios de escritura intensiva o en un entorno donde la carga en el almacenamiento de controlador de dominio es desconocida podría degradar la experiencia del cliente y debe realizar las horas de trabajo.

-   Afecta al tráfico de replicación es mínima, ya que generar los índices se produce localmente.

Para obtener más información, vea lo siguiente:

-   [Creación de aplicaciones habilitadas para directorio de Microsoft Active más eficientes](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Buscar en Active Directory Domain Services](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Atributos indizados](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)


## <a name="see-also"></a>Vea también
- [Servidores de Active Directory de optimización del rendimiento](index.md)
- [Consideraciones de hardware](hardware-considerations.md).
- [Colocación adecuada de los controladores de dominio y consideraciones de sitio](site-definition-considerations.md)
- [Solución de problemas de rendimiento de AD DS](troubleshoot.md) 
- [Capacity Planning for Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) (Planeamiento de la capacidad para Active Directory Domain Services)