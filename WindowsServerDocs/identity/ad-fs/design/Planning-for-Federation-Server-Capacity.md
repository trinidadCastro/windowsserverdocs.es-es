---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: "Planear la capacidad de servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-capacity"></a>Planear la capacidad de servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diseño de los servidores de federación capacidad te ayuda a calcular:  
  
-   Los factores que aumentan el tamaño de la base de datos de configuración de AD FS.  
  
-   Los requisitos de hardware adecuados para cada servidor de federación.  
  
-   El número de servidores de federación colocar en cada organización.  
  
Los servidores de federación emiten tokens de seguridad para los usuarios. Estos tokens se presentan al usuario de confianza para el consumo. Los servidores de federación emiten tokens de seguridad después de autenticar a un usuario o después de recibir un token de seguridad que se emitió previamente por un socio de servicios de federación. Cuando los usuarios iniciar sesión inicialmente en aplicaciones federadas o cuando sus tokens de seguridad venzan mientras que tienen acceso a aplicaciones federadas, se solicita un token de seguridad de un servicio de federación.  
  
Los servidores de federación están diseñados para dar cabida a configuraciones de conjunto de high\ disponibilidad de servidores que usan la tecnología \(NLB\) equilibrio de carga de red de Microsoft. Servidores de federación de configuración de un conjunto de dar servicio a solicitudes de forma independiente, sin tener acceso a los componentes de granja comunes para cada solicitud. Por lo tanto, hay poca sobrecarga implicados en la implementación de un servidor de federación el escalado.  
  
**Recomendaciones:**  
  
-   Para mission\ críticas o las implementaciones de disponibilidad de high\, te recomendamos que crees una granja de servidores de federación pequeño en cada organización de partner, con al menos dos servidores de federación cada granja de servidores, para proporcionar tolerancia a errores.  
  
-   Con la necesidad de alta disponibilidad y la facilidad de escalado de servidores de federación, el escalado es el método recomendado para controlar gran número de solicitudes por segundo para un servicio de federación particular. El escalado más allá de la configuración básica en esta guía es poco probable producir gran capacidad de ganancias de control.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Y el crecimiento AD FS configuración base de datos  
Por lo general, se considera que el tamaño de la base de datos de configuración de AD FS ser pequeño y no suelen ser un aspecto importante en las implementaciones de AD FS tamaño base de datos.  El tamaño exacto de la base de datos de configuración de AD FS puede dependen en gran medida el número de las relaciones de confianza y los metadatos relacionados con el trust\ asociados, como reclamaciones, reclamar reglas y supervisión de configuración configuradas para cada confianza. A medida que aumenta el número de entradas de confianza en la base de datos de configuración, por lo tanto, aumenta la necesidad de más espacio en disco.  
  
Para obtener información de implementación adicionales sobre la base de datos de configuración de AD FS, vea [consideraciones de la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisitos de espacio en disco, CPU y memoria  
Afortunadamente, los requisitos de espacio en disco, CPU y memoria para servidores de federación son reducidos, y no suelen ser un factor importante para ir en coche en las decisiones de hardware. Para obtener más información sobre los requisitos de hardware, consulta [Apéndice A: revisar AD FS requisitos](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> En las pruebas realizadas por el equipo de AD FS usando una granja de servidores de federación configurada con un servidor SQL dedicado para almacenar la base de datos de configuración de AD FS, la carga global en el servidor SQL solía ser bajo. En una prueba con un conjunto de servidor de federation\ four\ configurados para usar un único servidor SQL, el uso de CPU no ha sobrepasado 10% a pesar de pruebas que los servidores de federación de traen a la utilización de destino.  
  
## <a name="bk_estimatefs"></a>Calcular el número de servidores de federación de la organización  
En un esfuerzo para simplificar el proceso para servidores de federación de diseño de hardware, el equipo de producto de AD FS desarrolló la AD FS capacidad planeación tamaño hoja de cálculo. Esta hoja de cálculo de Excel incluye una funcionalidad similar calculator\ que tendrá los datos de uso que se espera que proporcionar acerca de los usuarios de la organización y devolver un número óptimo recomendado de los servidores de federación para el entorno de producción de AD FS.  
  
> [!NOTE]  
> El número de servidores de federación que te recomendará esta hoja de cálculo se basa en las especificaciones de hardware y de red que el equipo de AD FS usa durante las pruebas. Por lo tanto, debe comprender el número de servidores de federación que te recomendará la hoja de cálculo en este contexto.  Para obtener más información acerca de las especificaciones de usar durante las pruebas, consulta el tema titulado [planear la capacidad de servidor de AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Con la capacidad de FS AD tamaños hoja de cálculo de diseño  
Cuando usas esta hoja de cálculo, tendrás que seleccionar un valor \ (ya sea **40%**, **60%**, o **80%**\) que mejor represente el porcentaje del total de usuarios esperas enviará solicitudes de autenticación a los servidores de federación durante los períodos de uso.  
  
A continuación, tendrás que seleccionar un valor \ (ya sea **1 minuto**, **15 minutos**, o **1 hora**\) que mejor represente el intervalo de tiempo que espera el período de uso máximo al último. Por ejemplo, se puede calcular 40% como el valor para el número total de los usuarios que se inicie sesión en un período de 15 minutos, o 60% de los usuarios podrá iniciar una sesión en un plazo de 1 hora. Juntos, estos valores definen el perfil de carga máximo por el que se calcula la recomendación de tamaño.  
  
A continuación, deberás especificar el número total de usuarios que necesitarán solo sign\ en tener acceso a la aplicación con reconocimiento de claims\ de destino, en función de si los usuarios son:  
  
-   Iniciar sesión en Active Directory desde un equipo local que está conectado físicamente a la red corporativa \ (a través de authentication\ integrado de Windows)  
  
-   Registro en Active Directory de forma remota desde un equipo que no está conectado físicamente a la red corporativa \ (a través de Windows integrado autenticación o nombre de usuario y password\)  
  
-   Desde otra organización y se intenta acceder a la aplicación con reconocimiento de claims\ de destino de un socio de confianza  
  
-   Desde un proveedor de identidad SAML 2.0 e intentar acceder a la aplicación con reconocimiento de claims\ de destino  
  
#### <a name="how-to-use-this-spreadsheet"></a>Cómo usar esta hoja de cálculo  
Puedes usar los siguientes pasos para cada instancia de granja de servidor de federación que piensas implementar para determinar el número de servidores de federación recomendadas.  
  
1.  Descargar y, a continuación, abre el [AD FS capacidad planear tamaños hoja de cálculo para Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) o [AD FS capacidad planear tamaños hoja de cálculo para Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  En la celda a la derecha de la **durante el período de uso del sistema pico, esperar este porcentaje de Mis a los usuarios autenticarse** de la celda, haz clic en la celda y, a continuación, usa las flechas de drop\ abajo para seleccionar el nivel de utilización sistema estimada, ya sea **40%**, **60%** o **80%** para la implementación.  
  
3.  En la celda a la derecha de la **en el siguiente período de tiempo** de la celda, haz clic en la celda y, a continuación, usa las flechas de drop\ abajo para seleccionar **1 minuto**, **15 minutos**, o **1 hora** para seleccionar la duración de carga máximo.  
  
4.  En la celda a la derecha de la **ENTRAR número estimado de aplicaciones internas \ (como SharePoint \(2007 or 2010\) o reclamaciones web compatible applications\)** celda, escribe el número de aplicaciones internas que usarás en la organización.  
  
5.  En la celda a la derecha de la **ENTRAR número estimado de aplicaciones en línea \ (como Office 365 Exchange Online, SharePoint Online o Lync Online\)** celda, escribe el número de aplicaciones en línea o servicios que lo harás que se usan en la organización.  
  
6.  En la celda titulada **número de usuarios**, escribe un número en cada fila que se aplica a un escenario de aplicación de ejemplo, los usuarios se necesitan sign\ en el acceso a una única. Esta columna debe contener el número de usuarios definidos, no a los usuarios pico por segundo. Si la intentos de acceso de la aplicación primero deben someterse a la página de detección de dominio de inicio, escribe **Y**. Si no estás seguro de esta selección, escriba **Y**.  
  
7.  Revisa los siguientes valores que se proporcionan de recomendados:  
  
    1.  Para obtener el número total de los servidores de federación recomendadas, consulta la celda inferior derecha en la que está resaltada en gris.  
  
    2.  Para el número de servidores recomendada para cada escenario de aplicación de ejemplo, consulta la celda en la fila que está resaltada en color gris.  
  
> [!NOTE]  
> El valor que se calcularán automáticamente en la celda a la derecha de la celda titulada **número Total de los servidores de federación recomendado** en la parte inferior de la hoja de cálculo contiene una fórmula que un búfer de 20% adicionales, se agregará a la suma total de todos los valores de cada una de las filas individuales anterior. La fórmula agregada a la **número Total de los servidores de federación recomendado** celda se basa en este búfer al total recomendado número de servidores de federación implementados para que sea muy poco probable que la carga total de la granja nunca llegará a su punto de saturación.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
