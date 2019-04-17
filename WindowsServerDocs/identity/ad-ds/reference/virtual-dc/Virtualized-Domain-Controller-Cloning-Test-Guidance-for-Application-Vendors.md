---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: "Controlador de dominio virtualizada clonación Guía de prueba para proveedores de aplicaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 72c4e818f82d3252c45776b26fb59e095893f2c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Controlador de dominio virtualizada clonación Guía de prueba para proveedores de aplicaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica lo que los proveedores de aplicaciones deben tener en cuenta para ayudar a garantizar que su aplicación sigue funcionando como se esperaba después de que el controlador de dominio virtualizados (corriente continua) clonación proceso se completa. Trata los aspectos del proceso de clonación que los proveedores de aplicaciones de interés y escenarios que pueden justificar pruebas adicionales. Los proveedores de aplicaciones que han validado que su aplicación funciona en controladores de dominio virtualizada clonados se recomienda que muestra el nombre de la aplicación en la Comunidad de contenido en la parte inferior de este tema, junto con un vínculo al sitio web de la organización donde los usuarios pueden más información sobre la validación.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Información general de clonación virtualizada de DC  
El controlador de dominio virtualizada clonación proceso se describe con detalle en [Introducción a la virtualización de los servicios de dominio de Active Directory (AD DS) (nivel 100)](https://technet.microsoft.com/library/hh831734.aspx) y [virtualizados dominio controlador referencia técnica (nivel 300)](https://technet.microsoft.com/library/jj574214.aspx). Desde la perspectiva del proveedor de la aplicación, estas son algunas consideraciones a tener en cuenta al evaluar el efecto de clonación a la aplicación:  
  
-   El equipo original no se destruye. Permanece en la red, interactuar con los clientes. A diferencia de un cambio de nombre que se quitan los registros DNS del equipo original, permanecerán los registros originales para el controlador de dominio de origen.  
  
-   Durante el proceso de clonación, el nuevo equipo inicialmente se ejecuta durante un breve período de tiempo en la identidad del equipo antiguo hasta que el proceso de clonación se inicia y realiza los cambios necesarios. Las aplicaciones que crean registros acerca del host deben asegurarse de que el equipo clonado no sobrescribirá registros acerca del host original durante el proceso de clonación.  
  
-   La clonación es una funcionalidad específica de implementación solo virtualizada para controladores de dominio, no una extensión de propósito general para clonar otros roles de servidor. Algunos roles de servidor específicamente no se admiten para clonar:  
  
    -   Protocolo de configuración dinámica de Host (DHCP)  
  
    -   Servicios de certificados de Active Directory (AD CS)  
  
    -   Active Directory Rights Management Services (AD LDS)  
  
-   Como parte del proceso de clonación, se copia toda la VM que representa el controlador de dominio original, por lo que también se copia cualquier estado de la aplicación en esa máquina virtual. Validar que la aplicación se adapte a este cambio de estado del host local en el DC clonado, o si intervención se requiere, como un reinicio del servicio.  
  
-   Como parte de clonación, el controlador de dominio obtiene una identidad de la máquina nueva y disposiciones sí como una réplica de controlador de dominio de la topología. Valida la aplicación depende de la identidad del equipo, como su nombre, cuenta, SID y así sucesivamente. ¿Adapta automáticamente para informarle del cambio de la identidad del equipo en la copia de? Si esa aplicación almacena datos en caché, asegúrate de que no se basa en los datos de identidad del equipo pueden almacenarse en caché.  
  
## <a name="what-is-interesting-for-application-vendors"></a>¿Qué es interesante para los proveedores de aplicaciones?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Un controlador de dominio que se ejecuta la aplicación o servicio no se puede clonar hasta que la aplicación o servicio es:  
  
-   Agrega al archivo CustomDCCloneAllowList.xml mediante el cmdlet Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
- O bien-  
  
-   Quita el controlador de dominio  
  
La primera vez que el usuario ejecuta el cmdlet Get-ADDCCloningExcludedApplicationList, devuelve una lista de aplicaciones y servicios que se ejecutan en el controlador de dominio, pero no están en la lista de aplicaciones y servicios que son compatibles con clonación predeterminada. De manera predeterminada, el servicio o la aplicación no aparecerá. Para agregar tu servicio o aplicación a la lista de aplicaciones y servicios que pueden ser de forma segura clona, se ejecuta el usuario cmdlet Get-ADDCCloningExcludedApplicationList nuevo con la opción - GenerateXML con el fin de agregarla a la CustomDCCloneAllowList.xml del archivo. Para obtener más información, consulta [paso 2: cmdlet Get-ADDCCloningExcludedApplicationList ejecutar](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interacciones del sistema distribuido  
Por lo general servicios aislados en el equipo local pasan o generar un error cuando participan en clonación. Servicios distribuidos tienen por qué preocuparse por tener dos instancias del equipo host al mismo tiempo en la red durante un breve período de tiempo. Esto puede manifestarse como una instancia del servicio intentando extraer información desde un sistema de partner donde la copia se ha registrado como el nuevo proveedor de la identidad. O ambas instancias del servicio pueden incorporar información en la base de datos de AD DS al mismo tiempo con resultados diferentes. Por ejemplo, no es determinista cuando dos equipos que tengan el servicio de tecnologías de pruebas de Windows (WTT) se encuentran en la red con el controlador de dominio, se comunicará con qué equipo.  
  
Para el servicio de servidor DNS distribuido, el proceso de clonación cuidadosamente evita sobrescribir los registros DNS del controlador de dominio de origen cuando se inicia el controlador de dominio clone con una nueva dirección IP.  
  
No debería basarse en el equipo para quitar todas la antigua identidad hasta el final de clonación. Después de que el nuevo controlador de dominio se promueve dentro del contexto nuevo, selecciona Sysprep proveedores se ejecutan para limpiar el estado del equipo. Por ejemplo, en este punto es se quitarán los certificados del equipo antiguos y se cambian los secretos de criptografía que el equipo pueda acceder.  
  
El factor mayor que varía la duración de la clonación es cuántos objetos hay son replicar desde el PDC. Medios antiguos aumenta el tiempo necesario para clonación completa.  
  
Dado que tu servicio o aplicación es desconocida, queda ejecutando. El proceso de clonación no cambia el estado de los servicios no son de Windows.  
  
Además, el nuevo equipo tiene una dirección IP diferente que el equipo original. Estos comportamientos pueden provocar efectos secundarios para tu servicio o aplicación en función de cómo se comporta el servicio o aplicación en este entorno.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Escenarios adicionales sugeridos para las pruebas  
  
### <a name="cloning-failure"></a>Error clonación  
Los proveedores de servicios deben probar este escenario ya cuando se produce un error de clonación el equipo arranca en servicios de directorio reparación modo (DSRM), una forma de modo seguro. En este punto el equipo no ha finalizado la clonación. Puede haber cambiado algún estado y puede quedar algún estado del controlador de dominio original. Este escenario para comprender el impacto que puede tener en la aplicación de prueba.  
  
Para inducir una clonación falla, intenta clonar un controlador de dominio sin conceder permiso para clonar. En este caso, el equipo cambiaron solo las direcciones IP y seguir teniendo la mayor parte de su estado desde el controlador de dominio original. Para obtener más información acerca de cómo conceder un permiso de controlador de dominio para clonar, consulta [paso 1: conceder el controlador de dominio virtualizada de origen el permiso para clonar](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Emulador PDC clonación  
Proveedores de servicio y aplicación deben probar este escenario ya hay un reinicio adicional cuando se clona el emulador PDC. Además, la mayoría de clonación se realiza con una identidad temporal para permitir que la nueva copia interactuar con el emulador PDC durante el proceso de clonación.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Escritura en comparación con los controladores de dominio de solo lectura  
Deben probar los proveedores de servicio y aplicación clonación usando el mismo tipo de controlador de dominio (es decir, en un controlador de dominio grabable o de solo lectura) que el servicio está previsto para ejecutarse en.  
  


