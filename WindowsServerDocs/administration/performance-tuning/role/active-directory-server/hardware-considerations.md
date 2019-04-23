---
title: Consideraciones de hardware en la optimización del rendimiento de AD
description: Consideraciones de hardware en la optimización del rendimiento de AD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0f1aa1e3c07c5cb9238a332156abfec248e74176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866096"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Consideraciones de hardware en la optimización del rendimiento de ADDS 

>[!Important]
> El siguiente es un resumen de las recomendaciones más importantes y consideraciones para optimizar el hardware de servidor para las cargas de trabajo de Active Directory tratados en mayor profundidad en el [planear la capacidad de los servicios de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) artículo. Se recomienda encarecidamente no solo para revisar los lectores [planear la capacidad de los servicios de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) para un mayor conocimiento técnico y las implicaciones de estas recomendaciones.

## <a name="avoid-going-to-disk"></a>Evitar entrar en el disco

Active Directory almacena en caché como gran parte de la base de datos ya que permite la memoria. Al capturar páginas de memoria son órdenes de magnitud más rápido que va a medios físicos, si el medio es el eje o basado en SSD. Agregue más memoria para minimizar la E/S de disco.

-   Prácticas recomendadas de Active Directory se recomienda colocar suficiente RAM para cargar todo el DIT en memoria, además de admitir el sistema operativo y otras aplicaciones instaladas, como software antivirus, copia de seguridad, supervisión y así sucesivamente.

    -   Para conocer las limitaciones de las plataformas heredadas, vea [uso de memoria por el proceso de Lsass.exe en controladores de dominio que ejecutan Windows Server 2003 o Windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Usar la memoria\\duración en caché de modo de espera a largo plazo promedio (s) &gt; contador de rendimiento de 30 minutos.

-   Poner el sistema operativo, los registros y la base de datos en volúmenes separados. Si todos o la mayoría del DIT puede almacenarse en caché, una vez que la memoria caché está preparada y en un estado estable, este pasa a ser menos relevante y ofrece un poco más flexibilidad en el diseño de almacenamiento. En escenarios donde no se almacenan en caché todo el DIT, resulta más importante la importancia de dividir el sistema operativo y los registros de base de datos en volúmenes separados.

-   Normalmente, las proporciones de E/S en el DIT son unos 90% de lectura y escritura del 10%. Escenarios donde los volúmenes de E/S de escritura superan notablemente 10-20% se consideran escritura intensiva. Escenarios de escritura intensiva no se benefician enormemente de la memoria caché de Active Directory. Para garantizar la durabilidad transaccional de datos que se escriben en el directorio, Active Directory no realiza el almacenamiento en caché de escritura de disco. En su lugar, confirman todas las operaciones de escritura en el disco antes de devolver un estado de finalización correcta para una operación, a menos que haya una solicitud explícita no se debe hacer esto. Por lo tanto, la E/S de disco rápido es importante el rendimiento de las operaciones de escritura en Active Directory. Las siguientes son recomendaciones de hardware que podrían mejorar el rendimiento de estos escenarios:

    -   Controladoras RAID de hardware

    -   Aumentar el número de discos de baja latencia y alto RPM hospeda los archivos de DIT y registro

    -   Caché de escritura en el controlador

-   Revise el rendimiento del subsistema de disco individualmente para cada volumen. Mayoría de los escenarios de Active Directory es principalmente basado en lectura, por lo tanto son las más importantes inspeccionar las estadísticas en el volumen que hospeda el DIT. Sin embargo, no pase por alto el resto de las unidades, incluido el sistema operativo, de supervisión y las unidades de los archivos de registro. Para determinar si el controlador de dominio esté configurado correctamente para evitar que se está el cuello de botella de rendimiento de almacenamiento, hacer referencia a la sección en subsistemas de almacenamiento para obtener recomendaciones sobre el almacenamiento de los estándares. En muchos entornos, la filosofía es para asegurarse de que hay suficiente espacio en cabeza para dar cabida a los aumentos repentinos o picos de carga. Estos umbrales de advertencia se convierte en limitadas umbrales donde el espacio para dar cabida a los aumentos repentinos o picos de carga y se reduce la capacidad de respuesta del cliente. En pocas palabras, si se supera estos umbrales no es malo a corto plazo (5 a 15 minutos varias veces al día), pero un sistema que ejecuta sostenido con estos tipos de estadísticas no totalmente almacena en caché la base de datos y puede ser sobre impuestos y debe investigarse.

    -   Base de datos ==&gt; Instances(lsass/NTDSA)\\promedio de latencia de lecturas de base de datos de E/S &lt; 15 ms

    -   Base de datos ==&gt; Instances(lsass/NTDSA)\\lecturas de base de datos de E/S por segundo &lt; 10

    -   Base de datos ==&gt; Instances(lsass/NTDSA)\\latencia promedio de escrituras en registro de E/S &lt; 10 ms

    -   Base de datos ==&gt; Instances(lsass/NTDSA)\\registro E/S escrituras por segundo: informativo únicamente.

        Para mantener la coherencia de datos, todos los cambios deben escribirse en el registro. No hay ningún número correcto o incorrecto, es solo una medida de cuánto el almacenamiento es compatible con.

-   Planee las cargas de E/S de disco no esenciales, tales como exámenes antivirus y de copia de seguridad, para los períodos de carga punta. Además, usar las soluciones de copia de seguridad y antivirus que admiten la característica de E/S de prioridad baja introducida en Windows Server 2008 para reducir la competencia con necesidades de E/S de Active Directory.

## <a name="dont-over-tax-the-processors"></a>No impuestos en los procesadores

Procesadores que no tienen suficiente ciclos pueden provocar tiempos de espera largos sobre la obtención de subprocesos de sesión en el procesador para la ejecución. En muchos entornos, la filosofía es asegurarse de que hay suficiente espacio para dar cabida a los aumentos repentinos principal o los picos de carga para minimizar el impacto en la capacidad de respuesta de cliente en estos escenarios. En pocas palabras, si se supera el a los umbrales no es malo a corto plazo (5 a 15 minutos varias veces al día), pero no proporciona un sistema que ejecuta sostenido con estos tipos de estadísticas de cualquier encabezado de espacio suficiente para dar cabida a cargas anómalas y fácilmente se puede colocar en un s cargado escenario. Los períodos continuados superior a los umbrales de gastos de sistemas deben investigarse cómo reducir la carga del procesador.

-   Para obtener más información sobre cómo seleccionar un procesador, vea [optimización del rendimiento de Hardware de servidor](../../hardware/index.md).

-   Agregar hardware, optimizar la carga, dirigir a clientes en otro lugar o quitar la carga desde el entorno para reducir la carga de CPU.

-   Use la información de procesador (\_Total)\\% utilización del procesador &lt; contador de rendimiento del 60%.

## <a name="avoid-overloading-the-network-adapter"></a>Evitar la sobrecarga del adaptador de red

Al igual que con los procesadores, el uso del adaptador de red excesiva hará que largos tiempos de espera para el tráfico saliente a dedíquese a la red. Active Directory suele tener las solicitudes entrantes de pequeñas y relativamente a mucho mayores cantidades de datos devueltos en los sistemas cliente. Los datos enviados superan con creces los datos recibidos. En muchos entornos, la filosofía es para asegurarse de que hay suficiente espacio en cabeza para dar cabida a los aumentos repentinos o picos de carga. Este umbral es un umbral de advertencia donde el espacio para dar cabida a los aumentos repentinos o picos de carga se convierte en limitada y se reduce la capacidad de respuesta del cliente. En pocas palabras, no es malo a corto plazo (5 a 15 minutos varias veces al día) que supere los umbrales, pero es un sistema que ejecuta sostenido con estos tipos de estadísticas sobre impuestos y deben investigarse.

-   Para obtener más información sobre cómo optimizar el subsistema de red, consulte [optimización del rendimiento de red subsistemas](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Usar la interfaz de red comparar (\*)\\Bytes enviados/seg con interfaz de red (\*)\\contador de rendimiento de ancho de banda actual. La relación debe ser inferior al 60% utilizado.

## <a name="see-also"></a>Vea también
- [Servidores de Active Directory de optimización del rendimiento](index.md)
- [Consideraciones de LDAP](ldap-considerations.md)
- [Colocación adecuada de los controladores de dominio y consideraciones de sitio](site-definition-considerations.md)
- [Solución de problemas de rendimiento de ADDS](troubleshoot.md) 
- [Planear la capacidad de servicios de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)