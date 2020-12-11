---
description: 'Más información sobre: Guía de pruebas de clonación de controladores de dominio virtualizados para proveedores de aplicaciones'
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Guía de pruebas de clonación de controladores de dominio virtualizados para proveedores de aplicaciones
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 328bef8456a5b6d4955bf03463bb4a6fecf2201e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045083"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Guía de pruebas de clonación de controladores de dominio virtualizados para proveedores de aplicaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica qué proveedores de aplicaciones deben tener en cuenta para asegurarse de que su aplicación sigue funcionando según lo previsto después de que se complete el proceso de clonación del controlador de dominio virtualizado (DC). En él se tratan los aspectos del proceso de clonación que interesan a los proveedores y escenarios de aplicaciones que pueden garantizar pruebas adicionales. Los proveedores de aplicaciones que han validado que su aplicación funciona en controladores de dominio virtualizados que se han clonado se recomienda que muestren el nombre de la aplicación en el contenido de la comunidad en la parte inferior de este tema, junto con un vínculo al sitio web de su organización en el que los usuarios pueden obtener más información sobre la validación.

## <a name="overview-of-virtualized-dc-cloning"></a>Información general de la clonación de controladores de dominio virtualizados
El proceso de clonación del controlador de dominio virtualizado se describe en detalle en [Introducción a la virtualización de Active Directory Domain Services (AD DS) (nivel 100)](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md) y [referencia técnica del controlador de dominio virtualizado (nivel 300)](../../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). Desde la perspectiva de un proveedor de la aplicación, estas son algunas consideraciones que se deben tener en cuenta al evaluar el impacto de la clonación en la aplicación:

-   No se destruye el equipo original. Permanece en la red, interactuando con los clientes. A diferencia de un cambio de nombre en el que se quitan los registros DNS del equipo original, permanecen los registros originales del controlador de dominio de origen.

-   Durante el proceso de clonación, el nuevo equipo se ejecuta inicialmente durante un breve período de tiempo bajo la identidad del equipo anterior hasta que se inicia el proceso de clonación y realiza los cambios necesarios. Las aplicaciones que crean registros sobre el host deben asegurarse de que el equipo clonado no sobrescriba registros sobre el host original durante el proceso de clonación.

-   La clonación es una funcionalidad de implementación específica solo para controladores de dominio virtualizados, no una extensión de propósito general para clonar otros roles de servidor. Algunos roles de servidor no se admiten específicamente para la clonación:

    -   Protocolo de configuración dinámica de host (DHCP)

    -   Active Directory Certificate Services (AD CS)

    -   Active Directory Lightweight Directory Services (AD LDS)

-   Como parte del proceso de clonación, se copia toda la máquina virtual que representa el controlador de dominio original, por lo que también se copia cualquier estado de aplicación en esa máquina virtual. Compruebe que la aplicación se adapta a este cambio en el estado del host local en el controlador de dominio clonado, o si se requiere alguna intervención, como un reinicio del servicio.

-   Como parte de la clonación, el nuevo controlador de dominio obtiene una nueva identidad de máquina y se aprovisiona como un controlador de dominio de réplica en la topología. Compruebe si la aplicación depende de la identidad de la máquina, como su nombre, cuenta, SID, etc. ¿Se adapta automáticamente al cambio de la identidad del equipo en el clon? Si esa aplicación almacena en caché los datos, asegúrese de que no se base en los datos de identidad de la máquina que se pueden almacenar en caché.

## <a name="what-is-interesting-for-application-vendors"></a>¿Qué es interesante para los proveedores de aplicaciones?

### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml
No se puede clonar un controlador de dominio que ejecute su aplicación o servicio hasta que la aplicación o el servicio sea:

-   Agregado al archivo CustomDCCloneAllowList.xml mediante el cmdlet de Windows PowerShell Get-ADDCCloningExcludedApplicationList

O bien

-   Quitado del controlador de dominio

La primera vez que el usuario ejecute el cmdlet Get-ADDCCloningExcludedApplicationList, devolverá una lista de servicios y aplicaciones que se ejecutan en el controlador de dominio, pero que no están en la lista predeterminada de servicios y aplicaciones que se admiten para la clonación. De forma predeterminada, el servicio o la aplicación no aparecerán en la lista. Para agregar el servicio o la aplicación a la lista de aplicaciones y servicios que se pueden clonar de forma segura, el usuario vuelve a ejecutar Get-ADDCCloningExcludedApplicationList cmdlet con la opción-GenerateXML para agregarlo al archivo de CustomDCCloneAllowList.xml. Para obtener más información, vea [paso 2: ejecutar el cmdlet Get-ADDCCloningExcludedApplicationList](/powershell/module/addsadministration/get-addccloningexcludedapplicationlist).

### <a name="distributed-system-interactions"></a>Interacciones del sistema distribuido
Normalmente, los servicios aislados en el equipo local se superan o no al participar en la clonación. Los servicios distribuidos deben preocuparse de tener dos instancias del equipo host en la red simultáneamente durante un breve período de tiempo. Esto puede manifestarse como una instancia de servicio que intenta extraer información de un sistema asociado en el que la clonación se ha registrado como el nuevo proveedor de la identidad. O ambas instancias del servicio pueden introducir información en la base de datos de AD DS al mismo tiempo con resultados diferentes. Por ejemplo, no es determinista a qué equipo se comunicará cuando dos equipos que tienen el servicio de tecnologías de pruebas de Windows (WTT) están en la red con el controlador de dominio.

En el caso del servicio servidor DNS distribuido, el proceso de clonación evita con cuidado la sobrescritura de los registros DNS del controlador de dominio de origen cuando el controlador de dominio clonado se inicia con una nueva dirección IP.

No debe confiar en el equipo para quitar toda la identidad anterior hasta el final de la clonación. Después de promocionar el nuevo controlador de dominio en el nuevo contexto, seleccione los proveedores de Sysprep se ejecutan para limpiar el estado adicional del equipo. Por ejemplo, en este momento se quitan los certificados antiguos del equipo y se cambian los secretos de criptografía a los que puede tener acceso el equipo.

El factor más importante que varía el tiempo de la clonación es el número de objetos que se van a replicar desde el PDC. Los medios más antiguos aumentan el tiempo necesario para completar la clonación.

Dado que el servicio o la aplicación son desconocidos, se deja en ejecución. El proceso de clonación no cambia el estado de los servicios que no son de Windows.

Además, el nuevo equipo tiene una dirección IP diferente de la del equipo original. Estos comportamientos pueden provocar efectos secundarios en el servicio o la aplicación en función de cómo se comporte el servicio o la aplicación en este entorno.

## <a name="additional-scenarios-suggested-for-testing"></a>Escenarios adicionales sugeridos para las pruebas

### <a name="cloning-failure"></a>Error de clonación
Los proveedores de servicios deben probar este escenario porque cuando se produce un error en la clonación, el equipo arranca en el modo de reparación de servicios de directorio (DSRM), una forma de modo seguro. En este momento, el equipo no ha completado la clonación. Es posible que algunos Estados hayan cambiado y que algún estado permanezca en el controlador de dominio original. Pruebe este escenario para comprender qué impacto puede tener en la aplicación.

Para inducir un error de clonación, intente clonar un controlador de dominio sin concederle permiso para clonarse. En este caso, el equipo solo habrá cambiado las direcciones IP y seguirá teniendo la mayor parte de su estado del controlador de dominio original. Para obtener más información acerca de cómo conceder permiso para clonar un controlador de dominio, consulte [paso 1: conceder al controlador de dominio virtualizado de origen el permiso que se va a clonar](../../get-started/virtual-dc/virtualized-domain-controller-deployment-and-configuration.md).

### <a name="pdc-emulator-cloning"></a>Clonación del emulador de PDC
Los proveedores de servicios y aplicaciones deben probar este escenario, ya que hay un reinicio adicional cuando se clona el emulador de PDC. Además, la mayoría de la clonación se realiza en una identidad temporal para permitir que el nuevo clon interactúe con el emulador de PDC durante el proceso de clonación.

### <a name="writable-versus-read-only-domain-controllers"></a>Controladores de dominio de solo lectura y de escritura
Los proveedores de servicios y aplicaciones deben probar la clonación mediante el mismo tipo de controlador de dominio (es decir, en un controlador de dominio grabable o de solo lectura) en el que está previsto ejecutarse el servicio.
