---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planear la capacidad de los servidores de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847396"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planear la capacidad de los servidores de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
> [!NOTE]  
> El contenido proporcionado en este tema no refleja las pruebas reales que se realizó en los servidores que ejecutan Windows Server 2012. El tema se actualizará cuando se realicen las pruebas oportunas.  
  
Planear la capacidad de los servicios de federación de Active Directory \(AD FS\) es el proceso de previsión de los períodos de uso al servicio de federación y se planea o escala\-la implementación de servidor de AD FS para cumplir con las de carga requisitos.  
  
En esta sección se describe las instrucciones de implementación para el servidor de federación y el rol de servidor proxy de federación y se basa en pruebas de laboratorio que ha llevado a cabo el equipo de producto de AD FS en Microsoft. El propósito de este contenido es ayudarte a:  
  
-   Realizar una estimación aproximada las necesidades de hardware para la implementación de AD FS específica de su organización, como el número de servidores de AD FS.  
  
-   Proyecto con exactitud el uso máximo para el inicio de sesión\-en las solicitudes, planee el crecimiento y asegúrese de que la implementación de AD FS es capaz de manejar que uso máximo esperado.  
  
Antes de pasar a leer este contenido de planificación de capacidad, te recomendamos que completes primero las tareas en el orden que en aparecen en las dos tablas siguientes. En la primera de ellas encontrarás vínculos a tareas recomendadas que te servirán para tener un contexto relevante para este tema de planificación de capacidad.  
  
|Tarea recomendada|Descripción|Referencia|  
|--------------------|---------------|-------------|  
|Comprender los requisitos para implementar servidores de federación de AD FS y servidores proxy de federación|Repasa los requisitos de hardware y software importantes para implementar servidores de federación y servidores proxy de federación.|[Apéndice A: Revisar los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Seleccione el tipo de base de datos de configuración de AD FS que va a implementar en su organización|Antes de que puede empezar a usar datos de planificación de capacidad en esta sección, primero debe determinar qué configuración de AD FS base de datos de tipo va a implementar, ya sea Windows Internal Database \(WID\) o un lenguaje de consulta estructurado \(SQL\) base de datos.|[El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Consideraciones de topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Definir el tipo de diseño de topología que vas a usar con la nueva base de datos de configuración de AD FS seleccionada.|Cuando hayas decidido el tipo de base de datos de configuración de AD FS que vas a usar en la implementación, deberás sopesar qué topología de implementación es la que más se acerca para colocar los servidores de federación y los servidores proxy de federación en tu entorno de producción.|[Determinar la topología de implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Descripción de la clave AD FS relacionados con capacidad términos de planificación|Revisar las definiciones comunes del planeamiento de capacidad términos que se usan a lo largo de la discusión de planeamiento de la capacidad de AD FS.|Consulta la sección [Términos de planificación de capacidad de AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) de este tema|  
  
Una vez que hayas repasado el contenido de la tabla anterior, podrás pasar a realizar las tareas de requisitos previos de la siguiente tabla.  
  
|Tarea de requisito previo|Descripción|Referencia|  
|---------------------|---------------|-------------|  
|Descargue la hoja de cálculo de tamaño de planificación de capacidad de AD FS|La hoja de cálculo de tamaño de planificación de capacidad de AD FS puede ayudarle a determinar el número de servidores de federación necesarios para una implementación de granja de servidores de federación de AD FS. En el vínculo proporcionado para la siguiente tarea encontrarás instrucciones sobre cómo usar esta hoja de cálculo.|[Hoja de cálculo de AD FS planificación de capacidad](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Recopilar datos sobre el número de usuarios que necesiten inicio de sesión único\-en \(SSO\) acceso a las notificaciones de destino\-compatible con aplicaciones y los períodos de uso de máxima esperada asociados con este acceso|Los datos de usuario que recabes se usarán como los valores de entrada necesarios en el contexto de la hoja de cálculo de valoración de tamaño de planificación de capacidad de AD FS.|[Calcular el número de servidores de federación para su organización](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Hoja de cálculo AD FS planificación de capacidad para Windows Server 2016|Hoja de cálculo de planeamiento actualizada para Windows Server 2016|[AD FS Windows planear la capacidad de Server 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Términos de planificación de capacidad de AD FS  
En la tabla siguiente se describe los términos importantes que se usan con frecuencia en esta sección de la Guía de diseño de AD FS de planeamiento de capacidad. Para obtener una lista más completa de los términos de AD FS, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Término|Definición|  
|--------|--------------|  
|Usuarios simultáneos|Número estimado de usuarios que se espera envíen solicitudes al servicio en un periodo de tiempo determinado, que suele ser durante un pico de actividad.|  
|Usuarios activos|Promedio aproximado de usuarios que están activos en un sistema (si bien no necesariamente enviando solicitudes) durante un periodo de tiempo determinado.|  
|Usuarios definidos|Recuento máximo teórico de usuarios, basado normalmente en el número de usuarios que tienen cuentas definidas en el sistema.|  
|Solicitudes por segundo|El número de solicitudes bien enviadas por clientes \(cuando hablamos de la carga en un sistema\) o procesados por servidores \(cuando hablamos de rendimiento del servidor de\) en un segundo. Esta métrica se emplea al planear el procesador de servidores y la capacidad de memoria.|  
|Utilización y capacidad de respuesta del servidor de destino|Métricas de éxito relacionadas con un rendimiento del servidor aceptable. Por lo general, cuando la capacidad de respuesta está por debajo del valor de destino o la utilización está por encima, se considerará que hay una sobrecarga en el sistema y que se necesita más capacidad.|  
|Windows Internal Database \(WID\)|El valor predeterminado de AD FS configuración base de datos que puede utilizarse como alternativa a SQL Server en algunas implementaciones de AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Entorno de configuración empleado en las pruebas de AD FS  
En esta sección se describe el entorno de configuración que utilizó el equipo de producto de AD FS para realizar las pruebas. El equipo utilizó la siguiente configuración de hardware, software y red para recopilar los datos de rendimiento y escalabilidad en las pruebas del servidor de federación:  
  
-   Dual cuádruple 2,27 gigahercios \(GHz\) \(8 núcleos\)  
  
-   16\-GB DE RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Red Gigabit  
  
> [!NOTE]  
> Aunque se usó 16 GB de RAM en el servidor de federación durante las pruebas, se puede usar un tamaño de memoria más moderado, por ejemplo, 4 GB de RAM por servidor de federación para la mayoría de las implementaciones de AD FS. Las recomendaciones que se proporcionan en este AD FS planeamiento de capacidad, así como los resultados proporcionados por el AD FS capacidad planeación hoja de cálculo se basan en suposiciones que utilizará cada servidor de federación aproximadamente 's de 4 GB de RAM para la mayoría de producción de AD FS entornos.  
  
El equipo de producto empleó la siguiente configuración para recopilar datos de rendimiento y escalabilidad para las pruebas de servidor proxy de federación:  
  
-   Dual 2.24 de núcleo cuádruple GHz \(4 núcleos\)  
  
-   4\-GB DE RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Red Gigabit  
  
> [!NOTE]  
> Recomendaciones de capacidad para servidores de AD FS pueden variar considerablemente, dependiendo de las especificaciones que elija para que la configuración de hardware y de red que se usará en un entorno determinado. Como punto de referencia, las instrucciones de valoración de tamaño incluidas en este tema se basan en una previsión de uso del 80 por ciento de los equipos antes mencionados.  
  
## <a name="measure-ad-fs-server-capacity"></a>Medir la capacidad del servidor de AD FS  
Los componentes de hardware que suelen influir en el rendimiento y escalabilidad de los servidores son la CPU, la memoria, el disco y los adaptadores de red. Afortunadamente, cada uno de los componentes de AD FS requiere muy poca memoria y espacio en disco. La conectividad de red es un requisito ya consabido. Por lo tanto, las pruebas de carga realizadas en servidores de federación y en servidores proxy de federación se centran primordialmente en dos áreas para medir la capacidad de servidor:  
  
-   **Solicitudes máximas de AD FS por segundo:** El número de inicio de sesión\-en las solicitudes procesadas por segundo en los servidores de federación. Esta medida puede ayudar a establecer la cantidad de usuarios simultáneos que pueden iniciar sesión a la vez en un servidor. Esta medida se puede usar junto con la medida de consumo de CPU para conocer su efecto en el rendimiento.  
  
-   **Consumo de CPU:** porcentaje por el que se mide la capacidad de la CPU. Esta medida puede ayudarle a determinar la carga total de CPU que se produjeron en función del número de inicio de sesión entrante\-en solicitudes por segundo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Más información sobre la planificación de capacidad de AD FS  
Después de haber completado las tareas de requisitos previos y te hayas familiarizado con los términos relacionados y los requisitos de hardware, puede usar las siguientes contenido de planeación de capacidad adicional para ayudarle a determinar el número recomendado de servidores de AD FS necesarios para su despliegue:  
  
-   [Planear la capacidad del servidor de federación](Planning-for-Federation-Server-Capacity.md)  
  
-   [Planear la capacidad de Proxy de servidor de federación](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
