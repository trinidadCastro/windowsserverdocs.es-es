---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Guía de pruebas de clonación de controladores de dominio virtualizados para proveedores de aplicaciones
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b2303bc837cdaf9f6e7ebd4b3ccbf6c66aa7ad2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879346"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Guía de pruebas de clonación de controladores de dominio virtualizados para proveedores de aplicaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica lo que deben considerar los proveedores de aplicaciones para ayudar a garantizar que su aplicación sigue funcionando como se esperaba después de que el controlador de dominio virtualizado (DC) proceso de clonación se complete. Abarca los aspectos del proceso de clonación que los proveedores de aplicaciones de interés y escenarios que pueden justificar pruebas adicionales. Se recomienda para mostrar el nombre de la aplicación en el contenido de la Comunidad en la parte inferior de este tema, junto con un vínculo a proveedores de aplicaciones que se han validado que su aplicación funciona en controladores de dominio virtualizados que se hayan clonado a su sitio web de la organización donde los usuarios pueden obtener más información acerca de la validación.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Información general de la clonación de controlador de dominio virtualizados  
El proceso de clonación de controlador de dominio virtualizado se describe detalladamente en [Introducción a la virtualización de servicios de dominio de Active Directory (AD DS) (nivel 100)](https://technet.microsoft.com/library/hh831734.aspx) y [virtualizados técnica del controlador de dominio Referencia (nivel 300)](https://technet.microsoft.com/library/jj574214.aspx). Desde la perspectiva del proveedor de la aplicación, estas son algunas consideraciones a tener en cuenta al evaluar el impacto de la clonación se realiza en la aplicación:  
  
-   No se destruye el equipo original. Permanece en la red, interactuar con los clientes. A diferencia de un cambio de nombre que se quitan los registros DNS del equipo original, los registros para el controlador de dominio de origen originales permanecen.  
  
-   Durante el proceso de clonación, el nuevo equipo se ejecuta inicialmente durante un breve período de tiempo bajo la identidad del equipo antiguo hasta que el proceso de clonación se inicia y realiza los cambios necesarios. Las aplicaciones que creación registros sobre el host deben asegurarse de que el equipo clonado no sobrescribe los registros de host original durante el proceso de clonación.  
  
-   La clonación es una funcionalidad específica de implementación solo controladores de dominio virtualizados, no una extensión de propósito general para clonar otros roles de servidor. Algunos roles de servidor específicamente no se admiten para la clonación:  
  
    -   Protocolo de configuración dinámica de host (DHCP)  
  
    -   Active Directory Certificate Services (AD CS)  
  
    -   Active Directory Lightweight Directory Services (AD LDS)  
  
-   Como parte del proceso de clonación, se copia toda la máquina virtual que representa el controlador de dominio original, por lo que también se copia cualquier estado de la aplicación en esa máquina virtual. Validar que la aplicación se adapta a este cambio en el estado del host local en el controlador de dominio clonado, o si es necesaria, por ejemplo, un reinicio del servicio ninguna intervención.  
  
-   Como parte de la clonación, el nuevo controlador de dominio obtiene una nueva identidad de máquina y disposiciones sí misma como un controlador de dominio de réplica en la topología. Validar si la aplicación depende de la identidad, como su nombre, cuenta, SID y así sucesivamente. ¿Adaptan automáticamente para el cambio de identidad de máquina en el clon? Si esa aplicación se almacena en caché datos, asegúrese de que no se basa en los datos de identidad de máquina que pueden almacenarse en caché.  
  
## <a name="what-is-interesting-for-application-vendors"></a>¿Qué es interesante para los proveedores de aplicaciones?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
No se puede clonar un controlador de dominio que ejecuta la aplicación o servicio hasta que la aplicación o servicio es:  
  
-   Agrega al archivo CustomDCCloneAllowList.xml mediante el cmdlet Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
O bien:  
  
-   Quita el controlador de dominio  
  
La primera vez que el usuario ejecuta el cmdlet Get-ADDCCloningExcludedApplicationList, devuelve una lista de servicios y aplicaciones que se ejecutan en el controlador de dominio pero no están en la lista predeterminada de los servicios y aplicaciones que se admiten para la clonación. De forma predeterminada, el servicio o aplicación no aparecerá. Para agregar el servicio o aplicación a la lista de aplicaciones y servicios que pueden estar de forma segura clonado, se ejecuta el usuario cmdlet Get-ADDCCloningExcludedApplicationList nuevo con la opción - GenerateXML para poder agregarlo a CustomDCCloneAllowList.xml de archivos. Para obtener más información, consulte [paso 2: Ejecute el cmdlet Get-ADDCCloningExcludedApplicationList](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interacciones de sistemas distribuidos  
Normalmente services aisladas en el equipo local se aprueban o no cuando se participa en la clonación. Los servicios distribuidos tienen que preocuparse de tener dos instancias del equipo host en la red al mismo tiempo durante un breve período de tiempo. Esto es posible que el manifiesto de una instancia de servicio intentando extraer información de un sistema asociado que registró el clon como el nuevo proveedor de la identidad. O bien, ambas instancias del servicio pueden insertar información en la base de datos de AD DS al mismo tiempo con resultados diferentes. Por ejemplo, no es determinista cuando dos equipos que tienen servicios de tecnologías de las pruebas de Windows (WTT) se encuentran en la red con el controlador de dominio, se comunicarán con el equipo.  
  
Para el servicio servidor DNS distribuido, el proceso de clonación con cuidado evita sobrescribir los registros DNS del controlador de dominio de origen cuando el controlador de dominio clonado se inicia con una nueva dirección IP.  
  
No debe confiar en el equipo para quitar todos los de la identidad anterior hasta el final de la clonación. Tras promover el nuevo controlador de dominio dentro del contexto nuevo, seleccione los proveedores se ejecutan para limpiar el estado adicional del equipo de Sysprep. Por ejemplo, en este momento es se quitan los certificados del equipo antiguos y se cambian los secretos de criptografía que puede tener acceso el equipo.  
  
El mayor factor que varía la temporización de la clonación es cuántos objetos hay son replicar desde el PDC. Medios antiguos aumenta el tiempo necesario para completar la clonación.  
  
Dado que el servicio o aplicación es desconocido, se sigan ejecutándose. El proceso de clonación no cambia el estado de los servicios que no sean Windows.  
  
Además, el nuevo equipo tiene una dirección IP diferente del equipo original. Estos comportamientos pueden producir efectos a un servicio o aplicación dependiendo de cómo se comporta el servicio o aplicación en este entorno.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Escenarios adicionales sugeridos para las pruebas  
  
### <a name="cloning-failure"></a>Error de clonación  
Proveedores de servicios deben probar este escenario porque cuando se produce un error de clonación el equipo arranca en servicios de reparación de modo directorio (DSRM), una forma de modo seguro. En este momento el equipo no ha finalizado la clonación. Puede haber cambiado algún estado y algún estado puede permanecer en el controlador de dominio original. Probar este escenario para entender el impacto que puede tener en su aplicación.  
  
Para provocar un error de clonación, intentar clonar un controlador de dominio sin concederle permiso para clonarse. En este caso, el equipo sólo habrá cambiado las direcciones IP y aún tiene la mayor parte de su estado desde el controlador de dominio original. Para obtener más información acerca de cómo conceder un permiso de controlador de dominio que pueden clonarse, consulte [paso 1: Conceder el controlador de dominio virtualizado de origen el permiso para clonarse](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Emulador de PDC de clonación  
Los proveedores de servicio y de aplicación deben probar este escenario porque no hay un reinicio adicional cuando se clona el emulador de PDC. Además, la mayoría de la clonación se realiza bajo una identidad temporal para permitir que el nuevo clon interactuar con el emulador de PDC durante el proceso de clonación.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>La propiedad de escritura frente a los controladores de dominio de solo lectura  
Los proveedores de servicio y de aplicación deben probar la clonación mediante el mismo tipo de controlador de dominio (es decir, en un controlador de dominio de escritura o de solo lectura) que se ha planeado para ejecutarse en servicio.  
  


