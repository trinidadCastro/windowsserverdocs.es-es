---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planear la capacidad de los servidores de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 29881c667d52ef4e61edf0e76fc5bda3237d0235
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938194"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planear la capacidad de los servidores de AD FS



> [!NOTE]
> El contenido que se proporciona en este tema no refleja las pruebas reales realizadas en los servidores que ejecutan Windows Server 2012. El tema se actualizará cuando se realicen las pruebas oportunas.

El planeamiento de la capacidad de Servicios de federación de Active Directory (AD FS) \( AD FS \) es el proceso de previsión de períodos de uso máximos para el servicio de Federación y planeamiento o escalado \- vertical de la implementación de AD FS Server para cumplir esos requisitos de carga.

En esta sección se describen las instrucciones de implementación para los roles de servidor de Federación y servidor proxy de Federación, y se basa en las pruebas de laboratorio realizadas por el equipo de productos de AD FS de Microsoft. El propósito de este contenido es ayudarte a:

-   Calcule detenidamente las necesidades de hardware de la implementación de AD FS específica de su organización, como el número de servidores AD FS.

-   Proyectar con precisión el uso máximo esperado para \- las solicitudes de inicio de sesión, planear el crecimiento y asegurarse de que la implementación de AD FS sea capaz de controlar el uso máximo esperado.

Antes de pasar a leer este contenido de planificación de capacidad, te recomendamos que completes primero las tareas en el orden que en aparecen en las dos tablas siguientes. En la primera de ellas encontrarás vínculos a tareas recomendadas que te servirán para tener un contexto relevante para este tema de planificación de capacidad.

|Tarea recomendada|Descripción|Referencia|
|--------------------|---------------|-------------|
|Comprender los requisitos para implementar AD FS servidores de Federación y servidores proxy de Federación|Repasa los requisitos de hardware y software importantes para implementar servidores de federación y servidores proxy de federación.|[Apéndice A: Revisión de los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|
|Seleccione el tipo de AD FS base de datos de configuración que va a implementar en su organización|Antes de poder empezar a usar los datos de planeación de capacidad en esta sección, primero debe determinar qué tipo de base de datos de configuración de AD FS va a implementar, ya sea Windows Internal Database \( WID \) o una lenguaje de consulta estructurado \( SQL \) Database.|[El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<p>[Consideraciones sobre la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md)|
|Definir el tipo de diseño de topología que vas a usar con la nueva base de datos de configuración de AD FS seleccionada.|Cuando hayas decidido el tipo de base de datos de configuración de AD FS que vas a usar en la implementación, deberás sopesar qué topología de implementación es la que más se acerca para colocar los servidores de federación y los servidores proxy de federación en tu entorno de producción.|[Determinar la topología de la implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|
|Descripción de los términos de planificación de capacidad relacionados con la AD FS de claves|Revise las definiciones de los términos comunes de planeamiento de la capacidad que se usan en el AD FS debate de planeamiento de la capacidad.|Consulta la sección [Términos de planificación de capacidad de AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) de este tema|

Una vez que hayas repasado el contenido de la tabla anterior, podrás pasar a realizar las tareas de requisitos previos de la siguiente tabla.

|Tarea de requisito previo|Descripción|Referencia|
|---------------------|---------------|-------------|
|Descargar la hoja de cálculo de tamaño de planeación de capacidad de AD FS|La hoja de cálculo de tamaño de planeamiento de la capacidad de AD FS puede ayudarle a determinar el número de servidores de Federación necesarios para la implementación de una granja de servidores de Federación de AD FS. En el vínculo proporcionado para la siguiente tarea encontrarás instrucciones sobre cómo usar esta hoja de cálculo.|[Hoja de cálculo de planeamiento de capacidad de AD FS](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|
|Recopile datos sobre el número de usuarios que requerirán \- acceso de inicio de sesión único en \( SSO \) a la aplicación para notificaciones de destino \- y los períodos de uso máximos esperados asociados a este acceso|Los datos de usuario que recabes se usarán como los valores de entrada necesarios en el contexto de la hoja de cálculo de valoración de tamaño de planificación de capacidad de AD FS.|[Calcular el número de servidores de federación de la organización](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|
|Hoja de cálculo de planeamiento de la capacidad de AD FS para Windows Server 2016|Hoja de cálculo de planeación actualizada para Windows Server 2016|[AD FS el planeamiento de la capacidad de Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)

## <a name="ad-fs-capacity-planning-terms"></a><a name="bk_terms"></a>Términos de planificación de capacidad de AD FS
En la tabla siguiente se describen los términos importantes que se usan con frecuencia en esta sección de planeación de capacidad de la guía de diseño de AD FS. Para obtener una lista completa de los términos de AD FS, consulte Descripción de los [conceptos de AD FS clave](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).

|Término|Definición|
|--------|--------------|
|Usuarios simultáneos|Número estimado de usuarios que se espera envíen solicitudes al servicio en un periodo de tiempo determinado, que suele ser durante un pico de actividad.|
|Usuarios activos|Promedio aproximado de usuarios que están activos en un sistema (si bien no necesariamente enviando solicitudes) durante un periodo de tiempo determinado.|
|Usuarios definidos|Recuento máximo teórico de usuarios, basado normalmente en el número de usuarios que tienen cuentas definidas en el sistema.|
|Solicitudes por segundo|Número de solicitudes enviadas por los clientes \( al hablar sobre la carga en un sistema \) o procesadas por los servidores \( al hablar sobre el rendimiento del servidor \) en un segundo. Esta métrica se emplea al planear el procesador de servidores y la capacidad de memoria.|
|Utilización y capacidad de respuesta del servidor de destino|Métricas de éxito relacionadas con un rendimiento del servidor aceptable. Por lo general, cuando la capacidad de respuesta está por debajo del valor de destino o la utilización está por encima, se considerará que hay una sobrecarga en el sistema y que se necesita más capacidad.|
|Windows Internal Database \( WID\)|La base de datos de configuración de AD FS predeterminada que se puede usar como alternativa a SQL Server en determinadas implementaciones de AD FS.|

## <a name="configuration-environment-used-during-ad-fs-testing"></a>Entorno de configuración empleado en las pruebas de AD FS
En esta sección se describe el entorno de configuración que el equipo de producto de AD FS usó para realizar sus pruebas. El equipo utilizó la siguiente configuración de hardware, software y red para recopilar los datos de rendimiento y escalabilidad en las pruebas del servidor de federación:

-   Dual Quad Core 2,27 gigahercio \( GHz \) \( 8 núcleos\)

-   16 \- GB de RAM

-   Windows Server 2008 R2, Enterprise Edition

-   Red Gigabit

> [!NOTE]
> Aunque se usaron 16 GB de RAM en el servidor de Federación durante las pruebas, se puede usar un tamaño de memoria más moderado, como 4 GB de RAM por cada servidor de Federación, para la mayoría de las implementaciones de AD FS. Las recomendaciones que se proporcionan en esta AD FS el contenido de planeamiento de la capacidad junto con los resultados proporcionados por la hoja de cálculo de planeamiento de la capacidad AD FS se basan en suposiciones en las que cada servidor de Federación usará aproximadamente 4GB's de RAM para la mayoría de los entornos de producción de AD FS.

El equipo de producto empleó la siguiente configuración para recopilar datos de rendimiento y escalabilidad para las pruebas de servidor proxy de federación:

-   Dual Quad Core 2,24 GHz \( 4 núcleos\)

-   4 \- GB de RAM

-   Windows Server 2008 R2, Enterprise Edition

-   Red Gigabit

> [!NOTE]
> Las recomendaciones de capacidad para los servidores de AD FS pueden variar considerablemente en función de las especificaciones que elija para el hardware y la configuración de red que se van a usar en un entorno determinado. Como punto de referencia, las instrucciones de valoración de tamaño incluidas en este tema se basan en una previsión de uso del 80 por ciento de los equipos antes mencionados.

## <a name="measure-ad-fs-server-capacity"></a>Medir la capacidad del servidor de AD FS
Los componentes de hardware que suelen influir en el rendimiento y escalabilidad de los servidores son la CPU, la memoria, el disco y los adaptadores de red. Afortunadamente, cada uno de los componentes de AD FS requiere muy poca demanda de memoria y espacio en disco. La conectividad de red es un requisito ya consabido. Por lo tanto, las pruebas de carga realizadas en servidores de federación y en servidores proxy de federación se centran primordialmente en dos áreas para medir la capacidad de servidor:

-   **Solicitudes de AD FS máximas por segundo:** El número de solicitudes de inicio \- de sesión procesadas por segundo en los servidores de Federación. Esta medida puede ayudar a establecer la cantidad de usuarios simultáneos que pueden iniciar sesión a la vez en un servidor. Esta medida se puede usar junto con la medida de consumo de CPU para conocer su efecto en el rendimiento.

-   **Consumo de CPU:** Porcentaje por el que se mide la capacidad de la CPU. Esta medida puede ayudarle a determinar la carga de CPU total que se produjo en función del número de solicitudes de inicio de sesión entrantes \- por segundo.

## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Más información sobre la planificación de capacidad de AD FS
Después de haber completado las tareas de requisitos previos y de familiarizarse con los términos y requisitos de hardware relacionados, puede usar el siguiente contenido de planeación de capacidad adicional para ayudarle a determinar el número recomendado de servidores de AD FS necesarios para la implementación:

-   [Planear la capacidad de los servidores de federación](Planning-for-Federation-Server-Capacity.md)

-   [Planear la capacidad de los servidores proxy de federación](Planning-for-Federation-Server-Proxy-Capacity.md)

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
