---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planear la capacidad de AD FS Server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planear la capacidad de AD FS Server

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
> [!NOTE]  
> El contenido se proporciona en este tema no refleja la prueba real que se realizó en servidores que ejecutan Windows Server 2012. En este tema se actualizará una vez que se ha realizado las pruebas necesarios.  
  
Es de \(AD FS\) capacidad de diseño de los servicios de federación de Active Directory el proceso de previsión de los períodos de uso para el servicio de federación y planeación o scaling\ hasta la implementación del servidor de AD FS para satisfacerlos requisitos de carga.  
  
Esta sección describe las directrices de implementación para el servidor de federación y los roles de servidor proxy de federación y se basa en pruebas de laboratorio realizada por el equipo de producto de AD FS de Microsoft. El propósito de este contenido es que te ayudarán a:  
  
-   Estrechamente para calcular las necesidades de hardware para la implementación de AD FS específico de la organización, como el número de servidores de AD FS.  
  
-   El uso de máxima esperada para sign\ en las solicitudes, plan para el crecimiento del proyecto y asegúrate de que la implementación de AD FS es capaz de control que espera el uso máximo con precisión.  
  
Antes de continuar con este contenido de planeamiento de capacidad de lectura, te recomendamos que primero realices las tareas en el orden en que se muestra en las dos tablas siguientes. En la primera tabla, se proporcionan vínculos a tareas recomendadas que te ayudarán a proporcionan contexto relevante para esta explicación de planeamiento de capacidad.  
  
|Tarea recomendada|Descripción|Referencia|  
|--------------------|---------------|-------------|  
|Comprender los requisitos para la implementación de los servidores de federación de AD FS y los proxies de servidor de federación|Revisa importantes requisitos de hardware y software necesarios para implementar el servidor de federación y los proxies de servidor de federación.|[Apéndice A: revisar los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Selecciona el tipo de base de datos de configuración de AD FS que implementas en la organización|Antes de que puede empezar a usar los datos de planeamiento de capacidad de esta sección, primero tienes que determinar qué tipo de base de datos de configuración de AD FS se va a implementar Windows Internal Database \(WID\) o una base de datos de lenguaje de consulta estructurado \(SQL\).|[La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Consideraciones de la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinar el tipo de diseño de la topología para usar con la nueva selección de base de datos de configuración de AD FS|Después de decidir el tipo de base de datos de configuración de AD FS para usar en la implementación, tendrás que tener en cuenta qué topología de implementación coincide más estrechamente con donde tendrás que los servidores de federación de lugar y federación proxies de servidor en el entorno de producción.|[Determinar la topología de implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Comprender la clave AD FS relacionados de planeamiento de capacidad términos|Revisa las definiciones de términos que se usan en todo el análisis de planeamiento de capacidad de AD FS de planeamiento de capacidad comunes.|Consulta la sección titulada [términos de diseño de la capacidad de AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) en este tema|  
  
Una vez que hayas leído el contenido en la tabla anterior, ahora puede completar las tareas necesarias en la siguiente tabla.  
  
|Tarea de requisitos previo|Descripción|Referencia|  
|---------------------|---------------|-------------|  
|Descargar de la hoja de cálculo de tamaño de planeamiento de capacidad de FS de anuncios|La hoja de cálculo de tamaño de diseño de AD FS capacidad puede ayudar a determinar el número de servidores de federación necesarios para una implementación de granja de servidor de federación de AD FS. Instrucciones sobre cómo usar esta hoja de cálculo están disponibles en el vínculo que se indica a continuación para la próxima tarea.|[Hoja de cálculo de AD FS planeamiento de capacidad](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Recopilar datos sobre el número de usuarios que necesitarán solo sign\ \(SSO\) acceso a la aplicación con reconocimiento de claims\ de destino y el esperado uso períodos asociados con este acceso|Estos datos de usuario que se recopilan se usará para los valores de entrada necesarios dentro del contexto de la hoja de tamaño de planeación de AD FS capacidad.|[Calcular el número de servidores de federación de la organización](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Hoja de cálculo AD FS planeamiento de la capacidad para Windows Server 2016|Hoja de cálculo de diseño actualizada para Windows Server 2016|[AD FS Windows planeamiento de capacidad de servidor de 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Términos de planeación de AD FS capacidad  
La siguiente tabla describe los términos importantes que se usan con frecuencia en esta sección de la Guía de diseño de AD FS de planeamiento de capacidad. Para obtener una lista más completa de los términos de AD FS, consulta [comprensión clave AD FS conceptos](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Término|Definición|  
|--------|--------------|  
|Usuarios simultáneos|El número estimado de usuarios que se espera que envía solicitudes al servicio en un determinado período de tiempo, suele ser un período de máxima actividad.|  
|Usuarios activos|El número de promedio aproximado de los usuarios que están activos en un sistema, pero no necesariamente enviar solicitudes, durante un período de tiempo determinado.|  
|Usuarios definidos.|Un recuento teóricos máximo de usuarios, por lo general en función del número de usuarios que han definido las cuentas en el sistema.|  
|Solicitudes por segundo|El número de solicitudes o enviados por los clientes \ (cuando hablar sobre la carga en un FAT32\) o se procesan los servidores \ (cuando hablan throughput\ de servidor) en un segundo. Esta métrica se usa en la planeación de procesador del servidor y capacidad de memoria.|  
|La capacidad de respuesta del servidor de destino y la utilización de|Métricas de éxito que enlaza el intervalo de rendimiento aceptable del servidor. Por lo general, si la capacidad de respuesta esté por debajo de o utilización supera el destino, el sistema se considera sobrecarga y se requiere más capacidad.|  
|Windows Internal Database \(WID\)|AD FS configuración base de datos predeterminada que se puede usar como una alternativa a SQL Server en determinadas implementaciones de AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Entorno de configuración que se usan durante las pruebas de AD FS  
En esta sección se describe el entorno de configuración que el equipo de AD FS utiliza para realizar las pruebas. El equipo usa el siguiente hardware del equipo, software y configuración de red para recopilar datos de rendimiento y escalabilidad en pruebas del servidor de federación de:  
  
-   Dual Quad Core 2.27 gigahercio \(GHz\) \(8 cores\)  
  
-   16\ GB DE RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Red Gigabit  
  
> [!NOTE]  
> Aunque 16 GB de RAM se usó en el servidor de federación durante las pruebas, un tamaño de memoria más moderado, como 4 GB de RAM por el servidor de federación puede usarse para la mayoría de las implementaciones de AD FS. Las recomendaciones que se proporcionan en este AD FS planeamiento de capacidad contenido junto con los resultados proporcionados por la AD FS capacidad planeación hoja de cálculo se basan en los supuestos que cada servidor de federación usará aproximadamente 4 GB infantil de RAM para la mayoría de los entornos de producción de AD FS.  
  
El equipo de producto utiliza la siguiente configuración para recopilar datos de rendimiento y escalabilidad para el proxy de servidor de federación pruebas:  
  
-   Dual \(4 cores\) Quad Core 2.24 GHz  
  
-   4\ GB DE RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Red Gigabit  
  
> [!NOTE]  
> Recomendaciones de capacidad para servidores de AD FS pueden variar considerablemente, según las especificaciones que elijas para que la configuración de hardware y de red para usarse en un entorno determinado. Como un punto de referencia, la orientación de tamaño proporcionada en este contenido se basa en un destino de uso del 80% de los equipos mencionados anteriormente.  
  
## <a name="measure-ad-fs-server-capacity"></a>Capacidad del servidor de AD FS de medida  
Por lo general, los componentes de hardware que afectan a la escalabilidad y rendimiento del servidor son la CPU, memoria, disco y adaptadores de red. Afortunadamente, cada uno de los componentes de AD FS requiere muy poco demanda de memoria y espacio en disco. Conectividad de red es un requisito obvio. Por lo tanto, las pruebas de carga que se realizan en los servidores de federación y los proxies de servidor de federación nos centraremos en dos áreas principales para medir la capacidad del servidor:  
  
-   **Máximo de solicitudes de AD FS por segundo:** el número de solicitudes de sign\ que se procesan por segundo en los servidores de federación. Esta medición puede ayudarte a determinar el número de usuarios simultáneo puede iniciar sesión en un servidor determinado. Puedes usar esta medición junto con la medición de consumo de CPU para comprender el efecto de la medida en el rendimiento.  
  
-   **Consumo de CPU:** el porcentaje de por qué CPU se mide la capacidad. Esta medición puede ayudarte a determinar la carga de CPU general que se produjeron en función del número de solicitudes de inicio de sesión sign\ entrantes por segundo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Proseguir una lectura más información sobre el planeamiento de capacidad de AD FS  
Después de completar las tareas necesarias y ha familiarizarse con términos relacionados y los requisitos de hardware, puedes usar la capacidad adicional siguiente planificar contenido que te ayudarán a determinar el número de servidores de AD FS necesarios para la implementación recomendado:  
  
-   [Planear la capacidad de servidor de federación](Planning-for-Federation-Server-Capacity.md)  
  
-   [Planear la capacidad de Proxy del servidor de federación](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
