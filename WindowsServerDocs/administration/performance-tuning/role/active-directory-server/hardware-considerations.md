---
title: Consideraciones de hardware en la optimización del rendimiento de AD
description: Consideraciones de hardware en la optimización del rendimiento de AD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4d1e6c2744cfe0d16b034e6511144bef92a46b2e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866662"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Consideraciones de hardware en agrega optimización del rendimiento 

>[!Important]
> A continuación se ofrece un resumen de las recomendaciones y consideraciones clave para optimizar el hardware de servidor para Active Directory cargas de trabajo que se tratan con mayor profundidad en el artículo [planeación de la capacidad de Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) . Es muy recomendable que los lectores revisen el [planeamiento de la capacidad de Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) para obtener una mayor comprensión técnica e implicaciones de estas recomendaciones.

## <a name="avoid-going-to-disk"></a>Evitar ir al disco

Active Directory almacena en caché la gran parte de la base de datos que permite la memoria. La captura de páginas de la memoria es un orden de magnitud más rápido que ir a un medio físico, independientemente de si el medio está basado en el eje o en SSD. Agregue más memoria para minimizar la e/s de disco.

-   Active Directory prácticas recomendadas recomiendan poner suficiente RAM para cargar el DIT completo en la memoria, además de acomodar el sistema operativo y otras aplicaciones instaladas, como antivirus, software de copia de seguridad, supervisión, etc.

    -   Para conocer las limitaciones de las plataformas heredadas, consulte [uso de memoria por parte del proceso Lsass. exe en los controladores de dominio que ejecutan Windows server 2003 o windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Use el contador\\de rendimiento duración &gt; promedio de la caché en espera de memoria a largo plazo 30 minutos.

-   Coloque el sistema operativo, los registros y la base de datos en volúmenes independientes. Si todos o la mayoría del DIT se pueden almacenar en la memoria caché, una vez que la memoria caché esté preparada y bajo un estado estable, se hace menos pertinente y ofrece un poco más de flexibilidad en el diseño del almacenamiento. En escenarios en los que no se puede almacenar en caché el DIT completo, es más importante la importancia de dividir el sistema operativo, los registros y la base de datos en volúmenes independientes.

-   Normalmente, las relaciones de e/s con el DIT son aproximadamente del 90% de lectura y del 10% de escritura. Los escenarios en los que los volúmenes de e/s de escritura superan significativamente el 10%-20% se consideran de escritura intensivas. Los escenarios con mucha actividad de escritura no se benefician enormemente de la memoria caché de Active Directory. Para garantizar la durabilidad transaccional de los datos que se escriben en el directorio, Active Directory no realiza el almacenamiento en caché de escritura en disco. En su lugar, confirma todas las operaciones de escritura en el disco antes de devolver un estado de finalización correcta para una operación, a menos que haya una solicitud explícita para no hacerlo. Por lo tanto, la e/s de disco rápida es importante para el rendimiento de las operaciones de escritura en Active Directory. Las siguientes son recomendaciones de hardware que podrían mejorar el rendimiento de estos escenarios:

    -   Controladores RAID de hardware

    -   Aumentar el número de discos de baja latencia/alto RPM que hospedan los archivos de DIT y de registro

    -   Almacenamiento en caché de escritura en el controlador

-   Revise el rendimiento del subsistema de disco individualmente para cada volumen. La mayoría de los escenarios de Active Directory se basan principalmente en el nivel de lectura, por lo que es más importante inspeccionar las estadísticas del volumen que hospeda el DIT. Sin embargo, no pase por alto la supervisión del resto de las unidades, incluidos el sistema operativo y las unidades de archivos de registro. Para determinar si el controlador de dominio está configurado correctamente para evitar que el almacenamiento sea el cuello de botella para el rendimiento, consulte la sección sobre subsistemas de almacenamiento para conocer las recomendaciones de almacenamiento de estándares. En muchos entornos, la filosofía es asegurarse de que hay suficiente espacio suficiente para dar cabida a subidas o picos de carga. Estos umbrales son umbrales de advertencia en los que la sala principal para acomodar subidas o picos en la carga se vuelve restringida y la capacidad de respuesta del cliente se degrada. En Resumen, la superación de estos umbrales no es mala en el corto plazo (de 5 a 15 minutos varias veces al día). sin embargo, un sistema que se ejecuta de manera sostenida con este tipo de estadísticas no almacena por completo la base de datos y puede estar sobrecargado y debe investigarse.

    -   Database = =&gt; Instances (LSASS/NTDSA)\\lecturas de la base de datos &lt; de e/s 15ms de latencia media

    -   Base de datos&gt; = = instancias (LSASS/NTDSA)\\lecturas de base de datos &lt; de e/s 10 por segundo

    -   Base de datos&gt; = = instancias (LSASS/NTDSA)\\escrituras de &lt; registro de e/s de entrada promedio de 10 ms

    -   Database = =&gt; Instances (LSASS/NTDSA)\\escrituras de registro de e/s por segundo: solo informativo.

        Para mantener la coherencia de los datos, todos los cambios se deben escribir en el registro. No hay un número bueno o incorrecto aquí, solo es una medida de cuánto se admite el almacenamiento.

-   Planee cargas de e/s de disco no principales, como exámenes de copia de seguridad y antivirus, para períodos de carga no pico. Además, use soluciones de copia de seguridad y antivirus que admitan la característica de e/s de baja prioridad introducida en Windows Server 2008 para reducir la competencia con las necesidades de e/s de Active Directory.

## <a name="dont-over-tax-the-processors"></a>No sobrepasar impuestos por los procesadores

Los procesadores que no tienen suficientes ciclos libres pueden provocar tiempos de espera largos en la obtención de subprocesos en el procesador para su ejecución. En muchos entornos, la filosofía es asegurarse de que hay suficiente espacio suficiente para dar cabida a subidas o picos en la carga para minimizar el impacto en la capacidad de respuesta del cliente en estos escenarios. En Resumen, la superación de los umbrales siguientes no es mala en el corto plazo (de 5 a 15 minutos varias veces al día). sin embargo, un sistema que se ejecuta de forma sostenida con este tipo de estadísticas no proporciona ninguna sala principal para dar cabida a cargas anómalas y puede colocarse fácilmente en un exceso de impuestos cenario. Los sistemas que gastan períodos sostenidos por encima de los umbrales deben investigarse para reducir las cargas del procesador.

-   Para obtener más información sobre cómo seleccionar un procesador, consulte [Tuning performance for Server hardware](../../hardware/index.md).

-   Agregue hardware, optimice la carga, dirija a los clientes en otro lugar o quite la carga del entorno para reducir la carga de la CPU.

-   Use el contador de rendimiento\_información del\\procesador (total &lt; )% de uso del procesador 60%.

## <a name="avoid-overloading-the-network-adapter"></a>Evitar sobrecargar el adaptador de red

Al igual que con los procesadores, el uso excesivo de adaptadores de red provocará tiempos de espera prolongados para que el tráfico saliente llegue a la red. Active Directory tiende a tener solicitudes de entrada pequeñas y relativamente a cantidades significativamente mayores de datos que se devuelven a los sistemas cliente. Los datos enviados superan mucho los datos recibidos. En muchos entornos, la filosofía es asegurarse de que hay suficiente espacio suficiente para dar cabida a subidas o picos de carga. Este umbral es un umbral de advertencia en el que la sala principal para acomodar subidas o picos en la carga se vuelve restringida y la capacidad de respuesta del cliente se degrada. En Resumen, la superación de estos umbrales no es mala en el corto plazo (de 5 a 15 minutos varias veces al día). sin embargo, un sistema que se ejecuta de forma sostenida con este tipo de estadísticas es demasiado impuestos y debe investigarse.

-   Para obtener más información sobre cómo optimizar el subsistema de red, consulte [optimización del rendimiento de los subsistemas de red](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Use el contador de rendimiento\*comparar\\interfaz cluster () bytes enviados/seg\*.\\con el ancho de banda actual de interfaz cluster (). La relación debe ser inferior al 60% de uso.

## <a name="see-also"></a>Vea también
- [Optimizar el rendimiento de servidores Active Directory](index.md)
- [Consideraciones de LDAP](ldap-considerations.md)
- [Colocación adecuada de los controladores de dominio y consideraciones de sitio](site-definition-considerations.md)
- [Solución de problemas de rendimiento de AD DS](troubleshoot.md) 
- [Capacity Planning for Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) (Planeamiento de la capacidad para Active Directory Domain Services)