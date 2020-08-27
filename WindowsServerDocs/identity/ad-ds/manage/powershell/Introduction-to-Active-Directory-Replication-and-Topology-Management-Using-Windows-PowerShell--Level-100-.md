---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: Introduction to Active Directory Replication and Topology Management Using Windows PowerShell (Level 100)
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fd08814cd60f71f4ace9cfc0e374e80f54c6d89d
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941505"
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introduction to Active Directory Replication and Topology Management Using Windows PowerShell (Level 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows PowerShell para Active Directory ofrece la capacidad de administrar la replicación, los sitios, dominios y bosques, controladores de dominio y particiones. Los usuarios de herramientas de administración anteriores, como el complemento Sitios y servicios de Active Directory, y repadmin.exe, observarán que ahora está disponible funcionalidad similar en el contexto de Windows PowerShell para Active Directory. Además, los cmdlets son compatibles con los cmdlets existentes de Windows PowerShell para Active Directory, lo que crea una experiencia simplificada y permite a los clientes crear fácilmente scripts de automatización.

> [!NOTE]
> Los cmdlets para administración de la replicación y la topología de Windows PowerShell para Active Directory están disponibles en los siguientes entornos:
>
> -    Controlador de dominio de Windows Server 2012
> -    Windows Server 2012 con la Herramientas de administración remota del servidor para AD DS y AD LDS instaladas.
> -   Windows &reg; 8 con la herramientas de administración remota del servidor para AD DS y AD LDS instaladas.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Instalar el módulo de Active Directory para Windows PowerShell
El módulo de Active Directory para Windows PowerShell se instala de forma predeterminada cuando se instala el rol de servidor de AD DS en un servidor que ejecuta Windows Server 2012. No se necesita ningún paso adicional además de agregar el rol de servidor. También puede instalar el módulo de Active Directory en un servidor que ejecute Windows Server 2012 instalando el Herramientas de administración remota del servidor y puede instalar el módulo de Active Directory en un equipo que ejecute Windows 8 descargando e instalando las [herramientas administrativas del servidor remoto (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Consulte [Instrucciones](https://www.microsoft.com/download/details.aspx?id=28972)para obtener pasos de instalación.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Escenarios para probar los cmdlets de administración de la replicación y la topología de Windows PowerShell para Active Directory
Los siguientes escenarios están diseñados para que los administradores se familiaricen con los nuevos cmdlets de administración:

-   Obtener una lista de todos los controladores de dominio y sus sitios correspondientes

-   Administrar la topología de replicación

-   Ver la información de estado de replicación

## <a name="lab-requirements"></a>Requisitos del laboratorio

-   Dos controladores de dominio de Windows Server 2012: **DC1** y **DC2** que forman parte del dominio contoso.com y residen en el sitio corporativo dentro de ese dominio.

## <a name="view-domain-controllers-and-their-sites"></a>Ver controladores de dominio y sus sitios
En este paso usará el módulo de Active Directory para Windows PowerShell que permite ver los controladores de dominio existentes y la topología de replicación para el dominio.

Para completar los pasos de los procedimientos siguientes debe ser miembro del grupo Admins. del dominio o tener permisos equivalentes.

#### <a name="to-view-all-active-directory-sites"></a>Para ver todos los sitios de Active Directory

1.  En **DC1**, haga clic en **Windows PowerShell** en la barra de tareas.

2.  Escriba el siguiente comando:

    `Get-ADReplicationSite -Filter *`

    Devuelve información detallada sobre cada sitio. El parámetro `Filter` se usa en todos los cmdlets Active Directory de PowerShell para limitar la lista de objetos devueltos. En este caso, el asterisco (*) indica todos los objetos del sitio.

    > [!TIP]
    > Puede usar la tecla Tab para autocompletar los comandos de Windows PowerShell.
    >
    > Ejemplo: escriba `Get-ADRep` y presione la tecla TAB varias veces para omitir los comandos coincidentes hasta llegar a `Get-ADReplicationSite`. Autocompletar también funciona para los nombres de parámetro como `Filter`.

    Para dar formato a la salida desde el `Get-ADReplicationSite` comando como una tabla y limitar la visualización a campos específicos, puede canalizar la salida al `Format-Table` comando (o " `ft` " para abreviar):

    `Get-ADReplicationSite -Filter * | ft Name`

    Este comando devuelve una versión abreviada de la lista de sitios, que incluye solo el campo Name.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Para producir una tabla de todos los controladores de dominio

-   Escriba el siguiente comando en el símbolo del sistema del **módulo de Active Directory para Windows PowerShell**:

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Este comando devuelve los nombres de host de los controladores de dominio, así como sus asociaciones de sitio.

## <a name="manage-replication-topology"></a>Administrar la topología de replicación
En el paso anterior, después de ejecutar el comando, `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** se incluyó como parte del sitio **CORPORATE**. En los procedimientos a continuación, creará el sitio de una nueva sucursal, **BRANCH1**, creará un nuevo vínculo de sitio, establecerá la frecuencia de replicación y el costo del vínculo de sitio y moverá **DC2** a **BRANCH1**.

Para completar los pasos de los procedimientos siguientes debe ser miembro del grupo Admins. del dominio o tener permisos equivalentes.

#### <a name="to-create-a-new-site"></a>Para crear un sitio nuevo

-   Escriba el siguiente comando en el símbolo del sistema del **módulo de Active Directory para Windows PowerShell**:

    `New-ADReplicationSite BRANCH1`

    Este comando crea un nuevo sitio de sucursal, branch1.

#### <a name="to-create-a-new-site-link"></a>Para crear un vínculo de sitio nuevo

-   Escriba el siguiente comando en el símbolo del sistema del **módulo de Active Directory para Windows PowerShell**:

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Este comando creó el vínculo de sitio a **BRANCH1** y activó el proceso de notificación de cambios.

    > [!TIP]
    > Use la tecla TAB para autocompletar los nombres de parámetro como `-SitesIncluded` y `-OtherAttributes` en lugar de escribirlos manualmente.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Para establecer el costo del vínculo de sitio y la frecuencia de replicación

-   Escriba el siguiente comando en el símbolo del sistema del **módulo de Active Directory para Windows PowerShell**:

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Este comando establece el costo del vínculo de sitio a **BRANCH1** en **100** y establece la frecuencia de replicación con el sitio en **15 minutos**.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Para mover un controlador de dominio a un sitio distinto

-   Escriba el siguiente comando en el símbolo del sistema del **módulo de Active Directory para Windows PowerShell**:

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Este comando mueve el controlador de dominio, **DC2**, al sitio **BRANCH1**.

### <a name="verification"></a>Comprobación

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Para comprobar la creación de un sitio, el nuevo vínculo de sitio y el costo y la frecuencia de replicación

-   Haga clic en **Administrador del servidor**, en **Herramientas** y en **Sitios y servicios de Active Directory**, y compruebe lo siguiente:

    Compruebe que el sitio **BRANCH1** contenga todos lo valores correctos desde los comandos de Windows PowerShell.

    Compruebe que el vínculo de sitio **CORPORATE-BRANCH1** esté creado y se conecte con los sitios **BRANCH1** y **CORPORATE**.

    Compruebe que **DC2** esté ahora en el sitio **BRANCH1**. También puede abrir el **módulo de Active Directory para Windows PowerShell** y escribir el siguiente comando para comprobar que **DC2** esté ahora en el sitio **BRANCH1**: `Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Ver la información de estado de replicación
En los siguientes procedimientos, usará uno de los cmdlets de administración y replicación de Windows PowerShell para Active Directory, `Get-ADReplicationUpToDatenessVectorTable DC1`, para generar un informe de replicación simple usando la tabla de vectores de actualización mantenida por cada controlador de dominio. Esta tabla de vectores de actualización hace un seguimiento del USN de escritura de origen más alto visto desde cada controlador de dominio en el bosque.

Para completar los pasos de los procedimientos siguientes debe ser miembro del grupo Admins. del dominio o tener permisos equivalentes.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Para ver la tabla de vectores de actualización para un solo controlador de dominio

1.  Escriba el siguiente comando en el símbolo del sistema del **módulo de Active Directory para Windows PowerShell**:

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Esto muestra una lista de los USN más altos que **DC1** ve para cada controlador de dominio del bosque. El valor **Server** hace referencia al servidor que mantiene la tabla, en este caso, **DC1**. El valor **Partner** hace referencia al asociado de replicación (directo o indirecto) en el que se realizaron los cambios. El valor UsnFilter es el USN más alto que **DC1** ve desde Partner. Si se agrega un nuevo controlador de dominio al bosque, no aparecerá en la tabla de **DC1**hasta que **DC1** reciba un cambio que se originó en el nuevo dominio.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Para ver tabla de vectores de actualización para todos los controladores de dominio de un dominio

1.  Escriba el siguiente comando en el símbolo del sistema del módulo de Active Directory para Windows PowerShell:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Este comando reemplaza a **DC1** con `*`, y así recopila los datos de la tabla de vectores de actualización desde todos los controladores de dominio. Los datos se ordenan por **Partner** y **Server**, y después se muestran en una tabla.

    La ordenación permite comparar fácilmente el último USN visto por cada controlador de dominio para un asociado de replicación determinado. Esta es una manera rápida de comprobar que se está realizando la replicación en el entorno. Si la réplica funciona correctamente, los valores de UsnFilter notificados para un asociado de replicación determinado deben ser bastante similares en todos los controladores de dominio.

## <a name="see-also"></a>Consulte también
[Administración avanzada de la replicación y la topología de Active Directory con el nivel 200 de &#40;de Windows PowerShell&#41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


