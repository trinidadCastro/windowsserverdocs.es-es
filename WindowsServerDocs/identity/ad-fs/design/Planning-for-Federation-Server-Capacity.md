---
description: Más información acerca de cómo planear la capacidad del servidor de Federación
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Planificación de la capacidad de los servidores de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c81c589feb940fb686aa865c8368f126ab20562d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044643"
---
# <a name="planning-for-federation-server-capacity"></a>Planificación de la capacidad de los servidores de federación

El planeamiento de la capacidad de los servidores de Federación ayuda a calcular:

-   Qué factores aumentan el tamaño de la base de datos de configuración de AD FS.

-   Los requisitos de hardware adecuados para cada servidor de Federación.

-   El número de servidores de Federación que se colocarán en cada organización.

Los servidores de Federación emiten tokens de seguridad a los usuarios. Estos tokens se presentan a un usuario de confianza para su consumo. Los servidores de Federación emiten tokens de seguridad después de autenticar a un usuario o después de recibir un token de seguridad emitido previamente por un asociado Servicio de federación. Se solicita un token de seguridad de un Servicio de federación cuando los usuarios inician sesión inicialmente en aplicaciones federadas o cuando expiran los tokens de seguridad mientras tienen acceso a las aplicaciones federadas.

Los servidores de Federación están diseñados para adaptarse a las \- configuraciones de granja de servidores de alta disponibilidad que usan la tecnología NLB de equilibrio de carga de red de Microsoft \( \) . Los servidores de Federación de una configuración de granja pueden atender solicitudes de forma independiente, sin tener acceso a los componentes comunes de la granja para cada solicitud. Por lo tanto, el escalado horizontal de una implementación de servidor de Federación conlleva poca sobrecarga.

**Recomendaciones:**

-   En el caso de \- implementaciones críticas o de alta \- disponibilidad, se recomienda crear una pequeña granja de servidores de Federación en cada organización asociada, con al menos dos servidores de Federación por granja, para proporcionar tolerancia a errores.

-   Con la necesidad de alta disponibilidad y la facilidad de escalado horizontal de los servidores de Federación, el escalado horizontal es el método recomendado para controlar un número elevado de solicitudes por segundo para un Servicio de federación determinado. No es probable que el escalado más allá de la configuración base de esta guía genere mejoras importantes en el tratamiento de la capacidad.

## <a name="ad-fs-configuration-database-size-and-growth"></a>Tamaño y crecimiento de la base de datos de configuración AD FS
El tamaño de la base de datos de configuración de AD FS se suele considerar pequeño, y el tamaño de la base de datos no tiende a ser una consideración importante en las implementaciones de AD FS.  El tamaño exacto de la base de datos de configuración de AD FS puede depender en gran medida del número de relaciones de confianza y de los \- metadatos relacionados con la confianza asociada, como las notificaciones, las reglas de notificaciones y la configuración de supervisión establecida para cada confianza. A medida que crece el número de entradas de confianza en la base de datos de configuración, lo que hace es la necesidad de más espacio en disco.

Para más información sobre la implementación de la base de datos de configuración de AD FS, consulte consideraciones sobre la [topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md).

## <a name="memory-cpu-and-disk-space-requirements"></a>Requisitos de memoria, CPU y espacio en disco
Afortunadamente, los requisitos de memoria, CPU y espacio en disco de los servidores de Federación son modestos, por lo que no es probable que se produzcan en las decisiones de hardware. Para obtener más información acerca de los requisitos de hardware, consulte [Apéndice A: revisión de los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md).

> [!NOTE]
> En las pruebas realizadas por el equipo del producto de AD FS mediante una granja de servidores de Federación configurada con un SQL Server dedicado para almacenar la base de datos de configuración de AD FS, la carga global de la SQL Server se solía ser baja. En una prueba que usa una \- granja de servidores de Federación \- que se configuró para usar un único SQL Server, el uso de CPU no superó el 10% a pesar de las pruebas que hacían que los servidores de Federación hicieran uso de destino.

## <a name="estimate-the-number-of-federation-servers-for-your-organization"></a><a name="bk_estimatefs"></a>Calcular el número de servidores de federación de la organización
En un esfuerzo por simplificar el proceso de planeación de hardware para los servidores de Federación, el AD FS equipo de producto desarrolló la hoja de cálculo de tamaño de planeación de capacidad AD FS. Esta hoja de cálculo de Excel incluye \- una funcionalidad de calculadora como, que tomará los datos de uso esperados que proporcione sobre los usuarios de su organización y devolverá un número óptimo recomendado de servidores de Federación para su entorno de producción de AD FS.

> [!NOTE]
> El número de servidores de Federación que se recomienda para esta hoja de cálculo se basa en las especificaciones de hardware y de red que el equipo de producto de AD FS utilizó durante las pruebas. Por lo tanto, el número de servidores de Federación que recomendará la hoja de cálculo debe comprenderse dentro de este contexto.  Para obtener más información sobre las especificaciones usadas durante las pruebas, vea el tema titulado [planeación de la capacidad de AD FS Server](Planning-for-AD-FS-Server-Capacity.md).

### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Usar la hoja de cálculo de tamaño de planeamiento de la capacidad de AD FS
Cuando use esta hoja de cálculo, deberá seleccionar un valor del \( **40%**, **60%** o del **80%** \) que mejor represente el porcentaje del total de usuarios que espera enviará solicitudes de autenticación a los servidores de Federación durante períodos de uso máximo.

A continuación, deberá seleccionar un valor \( de **1 minuto**, **15 minutos** o **1 hora** \) que mejor represente la cantidad de tiempo que espera que dure el período de uso máximo. Por ejemplo, puede calcular el 40% como el valor del número total de usuarios que iniciarán sesión en un período de 15 minutos o que el 60% de los usuarios iniciará sesión en un período de 1 hora. Juntos, estos valores definen el perfil de carga máxima por el que se calculará la recomendación de ajuste de tamaño.

A continuación, deberá especificar el número total de usuarios que requerirán \- acceso de inicio de sesión único a la aplicación con reconocimiento de notificaciones de destino \- , en función de si los usuarios son:

-   Iniciar sesión en Active Directory desde un equipo local que está conectado físicamente a la red corporativa \( mediante la autenticación integrada de Windows\)

-   Iniciar sesión en Active Directory remotamente desde un equipo que no está conectado físicamente a la red corporativa \( mediante la autenticación integrada de Windows o el nombre de usuario y la contraseña\)

-   De otra organización y está intentando acceder a la aplicación compatible con notificaciones \- de destino desde un asociado de confianza

-   Desde un proveedor de identidad SAML 2,0 y está intentando obtener acceso a la aplicación con reconocimiento de notificaciones de destino \-

#### <a name="how-to-use-this-spreadsheet"></a>Cómo usar esta hoja de cálculo
Puede usar los pasos siguientes para cada instancia de granja de servidores de Federación que planea implementar para determinar el número recomendado de servidores de Federación.

1.  Descargue y abra la [hoja de cálculo AD FS Capacity Planning Sizing para Windows server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) o la [hoja de cálculo AD FS Capacity Planning Sizing para Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).

2.  En la celda situada a la derecha de **durante el período de uso máximo del sistema, espero que este porcentaje de la autenticación de los usuarios** sea la celda, haga clic en la celda y, a continuación, use las flechas desplegables \- para seleccionar el nivel de uso del sistema estimado, ya sea **40%**, **60%** o **80%** para la implementación.

3.  En la celda situada a la derecha de la celda en **el siguiente período de tiempo** , haga clic en la celda y, a continuación, use las flechas desplegables \- para seleccionar **1 minuto**, **15 minutos** o **1 hora** para seleccionar la duración de la carga máxima.

4.  En la celda situada a la derecha de la celda **Escriba el número estimado de aplicaciones internas \( como SharePoint \( 2007 o 2010 \) o de aplicaciones \) web para notificaciones** , escriba el número de aplicaciones internas que usará en su organización.

5.  En la celda situada a la derecha de la celda **Escriba el número estimado de aplicaciones en línea \( como Office 365 Exchange Online, SharePoint Online \) o Lync Online** , escriba el número de aplicaciones o servicios en línea que usará en su organización.

6.  En la celda titulada **número de usuarios**, escriba un número en cada fila que se aplique a un escenario de aplicación de ejemplo en el que los usuarios necesitarán \- acceso de inicio de sesión único. Esta columna debe contener el número de usuarios definidos, no los usuarios máximos por segundo. Si los intentos de acceso realizados a la aplicación primero deben pasar por la página de detección del dominio de inicio, escriba **Y**. Si no está seguro de esta selección, escriba **Y**.

7.  Revise los siguientes valores recomendados que se proporcionan:

    1.  Para obtener el número total de servidores de Federación recomendados, vea la celda inferior derecha que está resaltada en gris.

    2.  Para ver el número de servidores recomendados para cada escenario de aplicación de ejemplo, vea la celda de la fila resaltada en gris.

> [!NOTE]
> El valor que se calculará automáticamente en la celda situada a la derecha de la celda titulada **número total de servidores de Federación recomendados** en la parte inferior de la hoja de cálculo contiene una fórmula que agregará un búfer adicional del 20% al total de la suma de todos los valores de cada una de las filas individuales que lo preceden. La fórmula agregada al número **total de servidores de Federación recomienda** compilaciones de celdas en este búfer con el número total recomendado de servidores de Federación implementados para que sea muy improbable que la carga global de la granja alcance su punto de saturación.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
