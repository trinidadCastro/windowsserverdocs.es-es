---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Planear la capacidad de los servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839636"
---
# <a name="planning-for-federation-server-capacity"></a>Planear la capacidad de los servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Planear la capacidad de los servidores de federación ayuda a calcular:  
  
-   Los factores que aumentar el tamaño de la base de datos de configuración de AD FS.  
  
-   Los requisitos de hardware adecuado para cada servidor de federación.  
  
-   El número de servidores de federación va a colocar en cada organización.  
  
Los servidores de federación emiten tokens de seguridad a los usuarios. Estos tokens se presentan a un usuario de confianza para su uso. Los servidores de federación emiten tokens de seguridad después de autenticar un usuario o después de recibir un token de seguridad emitido anteriormente por un socio de servicio de federación. Cuando los usuarios inician sesión inicialmente las aplicaciones federadas o cuando sus tokens de seguridad expiran mientras que tienen acceso a aplicaciones federadas, se solicita un token de seguridad de un servicio de federación.  
  
Los servidores de federación están diseñados para dar cabida a alta\-configuraciones de conjunto de disponibilidad de servidores que usan Microsoft Network Load Balancing \(NLB\) tecnología. Los servidores de federación en una configuración de granja de servidores pueden atender solicitudes de forma independiente, sin tener acceso a los componentes comunes de la granja de servidores para cada solicitud. Por lo tanto, hay poca sobrecarga implicada en escalado horizontal de una implementación de servidores de federación.  
  
**Recomendaciones:**  
  
-   Para la misión\-crítica o alta\-implementaciones de disponibilidad, recomendamos que cree una granja de servidores de federación pequeño en cada organización asociada, con al menos dos servidores de federación por granja de servidores, para proporcionar tolerancia a errores.  
  
-   Con la necesidad de alta disponibilidad y la facilidad de escalado horizontal de los servidores de federación, el escalado horizontal es el método recomendado para administrar la gran cantidad de solicitudes por segundo de un servicio de federación determinado. Escalar verticalmente más allá de la configuración básica en esta guía es probable que genere la capacidad importante mejoras de control.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Crecimiento y el tamaño de base de datos de configuración de AD FS  
Por lo general se considera el tamaño de la base de datos de configuración de AD FS sea pequeño y tamaño de la base de datos no suelen ser un factor importante en las implementaciones de AD FS.  El tamaño preciso de la base de datos de configuración de AD FS puede dependen en gran medida el número de relaciones de confianza y la relación de confianza asociado\-metadatos relacionados, como notificaciones, las reglas de notificación y la supervisión de configuración para cada relación de confianza. A medida que crece el número de entradas de confianza en la base de datos de configuración, también lo hace la necesidad de más espacio en disco.  
  
Para obtener información de implementación adicionales acerca de la base de datos de configuración de AD FS, consulte [consideraciones de topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisitos de espacio en memoria, CPU y disco  
Afortunadamente, los requisitos de espacio de memoria, CPU y disco para los servidores de federación son modestos y no suelen ser un factor determinante en las decisiones de hardware. Para obtener más información acerca de los requisitos de hardware, consulte [Apéndice A: Revisar los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> En pruebas realizadas por el equipo de productos de AD FS con una granja de servidores de federación configurada con un servidor dedicado de SQL para almacenar la base de datos de configuración de AD FS, la carga general de SQL Server tendía a ser bajo. En una prueba mediante un cuatro\-federación\-granja de servidores que se configuró para usar un único servidor de SQL, el uso de CPU no supere el 10% a pesar de las pruebas que incluya los servidores de federación para el uso de destino.  
  
## <a name="bk_estimatefs"></a>Calcular el número de servidores de federación para su organización  
En un esfuerzo por simplificar el proceso para los servidores de federación de planeación de hardware, el equipo del producto AD FS desarrolló el AD FS capacidad planeación Sizing hoja de cálculo. Esta hoja de cálculo de Excel incluye calculadora\-como funciones que toman datos de uso que se espera que proporcione acerca de los usuarios de su organización y devolver un número óptimo recomendado de servidores de federación para su entorno de producción de AD FS .  
  
> [!NOTE]  
> El número de servidores de federación que le recomendará esta hoja de cálculo se basa en las especificaciones de hardware y de red que utilizó el equipo de producto de AD FS durante las pruebas. Por lo tanto, se debe entender el número de servidores de federación que le recomendará la hoja de cálculo dentro de este contexto.  Para obtener más información acerca de las especificaciones utilizadas durante las pruebas, vea el tema titulado [planear la capacidad de servidor de AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Uso de la hoja de cálculo de tamaño de planificación de capacidad de AD FS  
Al usar esta hoja de cálculo, deberá seleccionar un valor \(cualquier **40%**, **60%**, o **80%** \) que mejor representa el porcentaje de total de usuarios que se espera que las solicitudes de autenticación se enviará a los servidores de federación durante los períodos de uso.  
  
A continuación, deberá seleccionar un valor \(cualquier **1 minuto**, **15 minutos**, o **1 hora** \) que mejor representa el período de tiempo de espera el período de uso máximo a la última. Por ejemplo, podría calcular el 40% como el valor para el número total de usuarios que iniciará sesión en un plazo de 15 minutos, o que el 60% de los usuarios iniciará sesión en un plazo de 1 hora. Juntos, estos valores definen el perfil de carga máxima mediante el cual se calculará la recomendación de ajuste de tamaño.  
  
A continuación, deberá especificar el número total de usuarios que requiera inicio de sesión único\-en acceso a las notificaciones de destino\-aplicación compatible con, en función de si los usuarios son:  
  
-   Iniciar sesión en Active Directory desde un equipo local que está conectado físicamente a la red corporativa \(mediante la autenticación integrada de Windows\)  
  
-   Registro en Active Directory de forma remota desde un equipo que no está conectado físicamente a la red corporativa \(a través de Windows integrado autenticación o nombre de usuario y contraseña\)  
  
-   De otra organización y está intentando obtener acceso a las notificaciones de destino\-aplicación compatible con desde un asociado de confianza  
  
-   Desde un proveedor de identidades de SAML 2.0 y cuando esté intentando obtener acceso a las notificaciones de destino\-aplicación compatible con  
  
#### <a name="how-to-use-this-spreadsheet"></a>Cómo usar esta hoja de cálculo  
Puede usar los pasos siguientes para cada instancia de granja de servidores del servidor de federación que va a implementar para determinar el número recomendado de servidores de federación.  
  
1.  Descargue y, a continuación, abra el [AD FS capacidad Planning Sizing hoja de cálculo para Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) o [AD FS Planning Sizing hoja de cálculo capacidad para Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  En la celda situada a la derecha de la **durante el período de uso máximo del sistema, espero que este porcentaje de Mis usuarios autenticar** de celda, haga clic en la celda y, a continuación, utilice la lista desplegable\-flecha abajo para seleccionar el uso del sistema estimados nivel, ya sea **40%**, **60%** o **80%** para la implementación.  
  
3.  En la celda situada a la derecha de la **dentro del período de tiempo siguiente** de celda, haga clic en la celda y, a continuación, utilice la lista desplegable\-flecha abajo para seleccionar **1 minuto**, **de15minutos**, o **1 hora** para seleccionar la duración de la carga máxima.  
  
4.  En la celda situada a la derecha de la **ENTRAR número estimado de las aplicaciones internas \(como SharePoint \(2007 o 2010\) o notificaciones de aplicaciones web compatibles con\)**  de celda, escriba el número de las aplicaciones internas usará en su organización.  
  
5.  En la celda situada a la derecha de la **ENTRAR número estimado de las aplicaciones en línea \(, como Office 365 Exchange Online, SharePoint Online o Lync Online\)**  de celda, escriba el número de aplicaciones en línea o servicios que le usados en su organización.  
  
6.  En la celda titulada **número de usuarios**, escriba un número en cada fila que se aplica a un escenario de aplicación de ejemplo, los usuarios se necesita inicio de sesión único\-sobre el acceso a. Esta columna debe contener el número de usuarios definidos, no a los usuarios máximas por segundo. Si los intentos de acceso a la aplicación en primer lugar deben pasar a través de la página de detección del dominio de inicio, escriba **Y**. Si no está seguro de esta selección, escriba **Y**.  
  
7.  Revise los siguientes valores que se proporcionan recomendados:  
  
    1.  Para obtener el número total de servidores de federación recomendada, consulte la celda inferior derecha que aparece resaltada en gris.  
  
    2.  Para el número de servidores recomendada para cada escenario de aplicación de ejemplo, consulte la celda en la fila que está resaltada en gris.  
  
> [!NOTE]  
> El valor que se calculará automáticamente en la celda situada a la derecha de la celda titulada **número Total de servidores de federación recomendadas** en la parte inferior de la hoja de cálculo contiene una fórmula que se agregará un búfer adicional del 20% a la suma total de todos los valores de cada una de las filas individuales que le precede. La fórmula que se agrega a la **número Total de servidores de federación recomendadas** celda se basa en este búfer para el total de número recomendado de servidores de federación implementado para que sea muy poco probable que nunca alcance la carga total en la granja de servidores su punto de saturación.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
