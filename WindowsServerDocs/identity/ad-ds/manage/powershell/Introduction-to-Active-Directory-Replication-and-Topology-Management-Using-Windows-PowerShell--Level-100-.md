---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: "Introducción a la réplica de Active Directory y administración de la topología mediante Windows PowerShell (nivel 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 006179bb3220f7bccfc7510e1b8ef69678321074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introducción a la réplica de Active Directory y administración de la topología mediante Windows PowerShell (nivel 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows PowerShell para Active Directory incluye la capacidad para administrar la replicación, sitios, dominios y bosques, controladores de dominio y las particiones. Complemento de los usuarios anteriores de herramientas de administración, como los sitios de Active Directory y los servicios y repadmin.exe observarás que una funcionalidad similar ahora está disponible en el contexto de Active Directory de Windows PowerShell. Además, los cmdlets son compatibles con el existente de Windows PowerShell con los cmdlets de Active Directory, de, por consiguiente, crear una experiencia optimizada y permitir a los clientes crear fácilmente los scripts de automatización.

> [!NOTE]
> Windows PowerShell para la replicación de Active Directory y topología cmdlets están disponibles en los entornos siguientes:
> 
> -    Controlador de dominio de Windows Server 2012
> -    Windows Server 2012 con las herramientas de administración de servidor remoto para AD DS y AD LDS instalado.
> -   Windows&reg; 8 con las herramientas de administración de servidor remoto para AD DS y AD LDS instalado.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Instalar el módulo de Active Directory para Windows PowerShell
El Active Directory módulo de Windows PowerShell se instala de forma predeterminada cuando se instala el rol de servidor de AD DS en un servidor que ejecuta Windows Server 2012. Se requiere que no sea la adición del rol de servidor ningún paso adicional. También puedes instalar el módulo de Active Directory en un servidor que ejecuta Windows Server 2012 mediante la instalación de las herramientas de administración de servidor remoto, y puede instalar el módulo de Active Directory en un equipo que ejecute Windows 8 descargando e instalando el [remota herramientas administrativas del servidor (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Consulta [instrucciones](https://www.microsoft.com/download/details.aspx?id=28972)para ver los pasos de instalación.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Escenarios de pruebas de Windows PowerShell para los cmdlets de administración de replicación y topología de Active Directory
Los siguientes escenarios están diseñados para que los administradores familiarizarse con los cmdlets de administración nuevo:

-   Obtener una lista de todos los controladores de dominio y sus correspondientes sitios

-   Administrar la topología de replicación

-   Información y ver el estado de replicación

## <a name="lab-requirements"></a>Requisitos de laboratorio

-   Dos de los controladores de dominio de Windows Server 2012: **DC1** y **DC2** que forman parte del dominio contoso.com y residen en el sitio corporativo en ese dominio.

## <a name="view-domain-controllers-and-their-sites"></a>Ver sus sitios y los controladores de dominio
En este paso, usarás el Active Directory módulo de Windows PowerShell para ver los controladores de dominio existentes y la topología de replicación del dominio.

Para completar los pasos descritos en los siguientes procedimientos, debes ser miembro del grupo Domain Admins o tener permisos equivalentes.

#### <a name="to-view-all-active-directory-sites"></a>Para ver todos los sitios de Active Directory

1.  En **DC1**, haz clic en **Windows PowerShell** en la barra de tareas.

2.  Escribe el siguiente comando:

    `Get-ADReplicationSite -Filter *`

    Esto devuelve información detallada acerca de cada sitio. La `Filter`parámetro se usa a lo largo de los cmdlets de PowerShell de Active Directory para limitar la lista de los objetos devueltos. En este caso, el asterisco (*) indica todos los objetos de sitio.

    > [!TIP]
    > Puedes usar la tecla Tab para Autocompletar comandos en Windows PowerShell.
    > 
    > Ejemplo: Escriba `Get-ADRep`y presione la tecla Tab varias veces para pasar por los comandos coincidentes hasta llegar a `Get-ADReplicationSite`. Autocompletar funciona también para los nombres de los parámetros como `Filter`.

    Para dar formato a la salida de la `Get-ADReplicationSite`comando como una tabla y limitar la visualización a campos específicos, puede canalizar la salida a la `Format-Table`comando (o "`ft`" para abreviar):

    `Get-ADReplicationSite -Filter * | ft Name`

    Esto devuelve una versión más corta de la lista de sitios, incluyendo solo el campo de nombre.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Para producir una tabla de todos los controladores de dominio

-   Escribe el siguiente comando en el **módulo de Active Directory para Windows PowerShell** símbolo del sistema:

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Este comando devuelve que los controladores de dominio hospedar nombre, así como sus asociaciones de sitio.

## <a name="manage-replication-topology"></a>Administrar la topología de replicación
En el paso anterior, después de ejecutar el comando, `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** aparecía como parte de la **CORPORATE** sitio. En los siguientes procedimientos, vas a crear un nuevo sitio de sucursal, **BRANCH1**, crear un nuevo vínculo del sitio, establecer la frecuencia de replicación de costo del vínculo sitio y luego mover **DC2** a **BRANCH1**.

Para completar los pasos descritos en los siguientes procedimientos, debes ser miembro del grupo Domain Admins o tener permisos equivalentes.

#### <a name="to-create-a-new-site"></a>Para crear un nuevo sitio

-   Escribe el siguiente comando en el **módulo de Active Directory para Windows PowerShell** símbolo del sistema:

    `New-ADReplicationSite BRANCH1`

    Este comando crea el nuevo sitio de sucursal, branch1.

#### <a name="to-create-a-new-site-link"></a>Para crear un nuevo vínculo de sitio

-   Escribe el siguiente comando en el **módulo de Active Directory para Windows PowerShell** símbolo del sistema:

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Este comando crea el vínculo a sitios **BRANCH1** y activado el proceso de notificación de cambio.

    > [!TIP]
    > Usa la pestaña a nombres de Autocompletar parámetro como `-SitesIncluded`y `-OtherAttributes`en lugar de escribir en ellos manualmente.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Para establecer la frecuencia de replicación de costo del vínculo sitio

-   Escribe el siguiente comando en el **módulo de Active Directory para Windows PowerShell** símbolo del sistema:

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Este comando establece el costo del vínculo **BRANCH1** en **100** y establecer la frecuencia de replicación con el sitio para **15 minutos **.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Para mover un controlador de dominio a otro sitio

-   Escribe el siguiente comando en el **módulo de Active Directory para Windows PowerShell** símbolo del sistema:

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Este comando mueve el controlador de dominio, **DC2** a la **BRANCH1** sitio.

### <a name="verification"></a>Verificación

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Para comprobar la creación de sitios, el nuevo vínculo a sitios y la frecuencia de costo y la replicación

-   Haz clic en **administrador del servidor**, haz clic en **herramientas** y, a continuación, haz clic en **servicios y sitios de Active Directory** y comprueba lo siguiente:

    Comprueba que la **BRANCH1** sitio contiene todos los valores correctos de los comandos de Windows PowerShell.

    Comprueba la **CORPORATE BRANCH1** vínculo del sitio, se crea y se conecta el **BRANCH1** y **CORPORATE** sitios.

    Comprobar **DC2** está ahora en la **BRANCH1** sitio. Como alternativa, puedes abrir el **módulo Active Directory para Windows PowerShell** y escribe el siguiente comando para comprobar **DC2** está ahora en la **BRANCH1** sitio:`Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Consultar información de estado de replicación
En los siguientes procedimientos, usarás uno de Windows PowerShell para la replicación de Active Directory y cmdlets de administración, `Get-ADReplicationUpToDatenessVectorTable DC1`, para generar un informe de replicación simple con la tabla de vector de actualización mantenida cada controlador de dominio. En esta tabla de vector grado realiza un seguimiento de la escritura USN original visto cada controlador de dominio del bosque.

Para completar los pasos descritos en los siguientes procedimientos, debes ser miembro del grupo Domain Admins o tener permisos equivalentes.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Para ver la tabla de vector de actualización para un controlador de dominio

1.  Escribe el siguiente comando en el **módulo de Active Directory para Windows PowerShell** símbolo del sistema:

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Esto muestra una lista de los más altos USN visto por **DC1** para cada controlador de dominio del bosque. La **Server** valor hace referencia al servidor de mantenimiento de la tabla, en este caso **DC1**. La **Partner** valor hace referencia a los asociados de replicación (directo o indirecto) en el que se realizaron cambios. El valor de UsnFilter es el mayor USN visto por **DC1** de socio. Si se agrega un nuevo controlador de dominio en el bosque, no aparecerá en **DC1**de tabla hasta **DC1** recibe un cambio que se origina desde el nuevo dominio.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Para ver la tabla de vector de actualización para todos los controladores de dominio en un dominio

1.  En el módulo de Active Directory para Windows PowerShell símbolo del sistema, escribe el siguiente comando:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Este comando reemplaza **DC1** con `*`, por lo tanto, recopilar los datos de la tabla de vector de actualización de todos los controladores de dominio. Los datos se ordenan por **Partner** y **Server** y, a continuación, se muestran en una tabla.

    La ordenación te permite comparar fácilmente el último USN visto por cada controlador de dominio para un duplicador determinado. Esto es una forma rápida de comprobar que la replicación se lleva a cabo a través de su entorno. Si la replicación funciona correctamente, los valores de UsnFilter notificados para un duplicador determinado deben ser bastante similares en todos los controladores de dominio.

## <a name="see-also"></a>Consulta también
[Avanzadas de réplica de Active Directory y administración de la topología mediante Windows PowerShell & #40; Nivel 200 & #41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


