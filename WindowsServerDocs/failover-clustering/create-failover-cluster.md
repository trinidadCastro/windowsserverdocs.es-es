---
title: Crear un clúster de conmutación por error
description: Cómo crear un clúster de conmutación por error para Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843296"
---
# <a name="create-a-failover-cluster"></a>Crear un clúster de conmutación por error

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

En este tema se muestra cómo crear un clúster de conmutación por error a través del complemento Administrador de clústeres de conmutación por error o bien a través de Windows PowerShell. Este tema se centra en una implementación típica, en la que los objetos de equipo para el clúster y sus roles en clúster asociados se crean en los Servicios de dominio de Active Directory (AD DS). Si va a implementar un clúster de espacios de almacenamiento directo, en su lugar ve [implementar espacios de almacenamiento directo](../storage/storage-spaces/deploy-storage-spaces-direct.md).

También puede implementar un clúster desconectado de Active Directory. Este método de implementación permite crear un clúster de conmutación por error sin permisos para crear objetos de equipo en AD DS y sin necesidad de solicitar que los objetos de equipo estén preconfigurados en AD DS. Esta opción solo está disponible a través de Windows PowerShell, y se recomienda únicamente para casos específicos. Para obtener más información, consulte [Implementar un clúster desconectado de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Lista de comprobación: Crear un clúster de conmutación por error

|Estado|Tarea|Referencia|
|:---:|---|---|
|☐|Comprobar los requisitos previos|[Compruebe los requisitos previos](#verify-the-prerequisites)|
|☐|Instalar la característica de clúster de conmutación por error en todos los servidores que desees incluir como nodo de clúster|[Instalar la característica clúster de conmutación por error](#install-the-failover-clustering-feature)|
|☐|Ejecutar el Asistente para validación de clústeres a fin de validar la configuración|[Validar la configuración](#validate-the-configuration)|
|☐|Ejecutar el Asistente para crear clúster a fin de crear el clúster de conmutación por error|[Crear el clúster de conmutación por error](#create-the-failover-cluster)|
|☐|Crear roles en clúster para hospedar las cargas de trabajo de clúster|[Crear roles en clúster](#create-clustered-roles)|

## <a name="verify-the-prerequisites"></a>Comprobar los requisitos previos

Antes de comenzar, comprueba los siguientes requisitos previos:

- Asegúrate de que todos los servidores que deseas agregar como nodos de clúster ejecuten la misma versión de Windows Server.
- Revisa los requisitos de hardware para asegurarte de que tu configuración sea compatible. Para obtener más información, consulta [Requisitos de hardware de los clústeres de conmutación por error y opciones de almacenamiento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Si va a crear un clúster de espacios de almacenamiento directo, consulte [requisitos de hardware de espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Para agregar almacenamiento en clúster durante la creación del clúster, asegúrese de que todos los servidores pueden tener acceso al almacenamiento. (También puedes agregar el almacenamiento en clúster después de crear el clúster).
- Asegúrate de que todos los servidores que deseas agregar como nodos de clúster estén unidos al mismo dominio de Active Directory.
- (Opcional) Crea una unidad organizativa (OU) y mueve a dicha OU las cuentas de equipo para los servidores que deseas agregar como nodos de clúster. Se recomienda colocar los clústeres de conmutación por error en su propia OU en AD DS. De este modo, podrás controlar mejor qué configuración de directiva de grupo o qué configuración de plantilla de seguridad afecta a los nodos de clúster. El hecho de aislar los clústeres en su propia OU también ayuda a evitar la eliminación accidental de los objetos de equipo de clúster.

Además, comprueba los siguientes requisitos de la cuenta:

- Asegúrate de que la cuenta que deseas usar para crear el clúster sea un usuario de dominio con derechos de administrador en todos los servidores que quieres agregar como nodos de clúster.
- Asegúrate de que se cumpla una de las siguientes condiciones:
    - El usuario que crea el clúster tiene el permiso **Crear objetos de equipo** en la OU o el contenedor donde residen los servidores que conformarán el clúster.
    - Si el usuario no tiene el permiso **Crear objetos de equipo**, solicita a un administrador de dominio que preconfigure un objeto de equipo de clúster para el clúster. Para obtener más información, consulta [Preconfigurar objetos de equipo de clúster en Servicios de dominio de Active Directory](prestage-cluster-adds.md).

>[!NOTE]
>Este requisito no es aplicable si desea crear un clúster desconectado de Active Directory en Windows Server 2012 R2. Para obtener más información, consulte [Implementar un clúster desconectado de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Instalar la característica de clúster de conmutación por error

Debes instalar la característica de clúster de conmutación por error en todos los servidores que desees incluir como nodo de clúster de conmutación por error.

### <a name="install-the-failover-clustering-feature"></a>Instalar la característica de clúster de conmutación por error

1. Inicie el Administrador del servidor.
2. En el **administrar** menú, seleccione **agregar Roles y características**.
3. En el **antes de comenzar** página, seleccione **siguiente**.
4. En el **Seleccionar tipo de instalación** , seleccione **instalación basada en roles o basada en características**y, a continuación, seleccione **siguiente**.
5. En el **Seleccionar servidor de destino** , seleccione el servidor donde desea instalar la característica y, a continuación, seleccione **siguiente**.
6. En el **seleccionar roles de servidor** , seleccione **siguiente**.
7. En la página **Seleccionar características**, activa la casilla **Clúster de conmutación por error**.
8. Para instalar las herramientas de administración de clúster de conmutación por error, seleccione **agregar características**y, a continuación, seleccione **siguiente**.
9. En el **Confirmar selecciones de instalación** página, seleccione **instalar**.
<br>La característica de clústeres de conmutación por error no requiere reiniciar el servidor.

10. Cuando se complete la instalación, seleccione **cerrar**.
11. Repite este procedimiento en todos los servidores que desees incluir como nodo de clúster de conmutación por error.

>[!NOTE]
>Una vez instalada la característica de clústeres de conmutación por error, recomendamos aplicar las últimas actualizaciones de Windows Update. Además, para un clúster de conmutación por error basados en Windows Server 2012, revise el [revisiones y actualizaciones recomendadas para clústeres de conmutación por error basados en Windows Server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) Microsoft Support artículo e instalar las actualizaciones necesarias.

## <a name="validate-the-configuration"></a>Validar la configuración

Antes de crear el clúster de conmutación por error, recomendamos encarecidamente validar la configuración para asegurarse de que el hardware y sus valores sean compatibles con los clústeres de conmutación por error. Microsoft es compatible con una solución de clúster únicamente si el conjunto de la configuración supera todas las pruebas de validación y si todo el hardware está certificado para la versión de Windows Server en la que se ejecutan los nodos de clúster.

>[!NOTE]
>Debes tener por lo menos dos nodos para ejecutar todas las pruebas. Si solo tienes uno, no podrán ejecutarse muchas de las pruebas de almacenamiento más importantes.

### <a name="run-cluster-validation-tests"></a>Ejecutar pruebas de validación de clúster

1. En un equipo que tenga las Herramientas de administración de clústeres de conmutación por error instaladas desde las Herramientas de administración remota del servidor, o en un servidor donde hayas instalado la característica de clústeres de conmutación por error, inicia el Administrador de clústeres de conmutación por error. Para hacer esto en un servidor, inicie el administrador del servidor y, a continuación, en el **herramientas** menú, seleccione **Administrador de clústeres de conmutación por error**.
2. En el **Administrador de clústeres de conmutación por error** panel, en **administración**, seleccione **validar configuración**.
3. En el **antes de comenzar** página, seleccione **siguiente**.
4. En el **seleccionar servidores o un clúster** página, en el **escriba el nombre** cuadro, escriba el nombre NetBIOS o el nombre de dominio completo de un servidor que va a agregar como un nodo de clúster de conmutación por error y, a continuación, seleccione **Agregar**. Repite este paso para todos los servidores que quieras agregar. Para agregar varios servidores al mismo tiempo, separa los nombres con una coma o un punto y coma. Por ejemplo, escribe los nombres con el formato *server1.contoso.com, server2.contoso.com*. Cuando haya terminado, seleccione **siguiente**.
5. En el **opciones de pruebas** página, seleccione **ejecutar todas las pruebas (recomendadas)** y, a continuación, seleccione **siguiente**.
6. En el **confirmación** página, seleccione **siguiente**.

    La página Validación muestra el estado de las pruebas en ejecución.
7. En la página **Resumen**, realiza una de las acciones siguientes:
    
      - Si los resultados indican que las pruebas se completaron correctamente y la configuración es adecuada para los clústeres, y desea crear el clúster inmediatamente, asegúrate de que el **crear el clúster ahora con los nodos validados** comprobar casilla está seleccionada y, a continuación, seleccione **finalizar**. Después, continúa con el paso 4 del procedimiento [Crear el clúster de conmutación por error](#create-the-failover-cluster).
      - Si los resultados indican que había advertencias o errores, seleccione **Ver informe** para ver los detalles y determinar qué problemas deben corregirse. Ten en cuenta que una advertencia sobre una prueba de validación en particular indica que ese aspecto del clúster de conmutación por error puede ser compatible, pero podría no cumplir los procedimientos recomendados.
        
        >[!NOTE]
        >Si recibes una advertencia sobre la prueba Validar reserva persistente de espacios de almacenamiento, consulta la entrada de blog [La advertencia sobre la validación de un clúster de conmutación por error de Windows indica que los discos no son compatibles con las reservas persistentes de espacios de almacenamiento](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) para obtener más información.

Para obtener más información sobre las pruebas de validación de hardware, consulta [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Crear el clúster de conmutación por error

Para completar este paso, asegúrate de que la cuenta de usuario con la que inicias sesión cumpla los requisitos indicados en la sección [Comprobar los requisitos previos](#verify-the-prerequisites) de este tema.

1. Inicie el Administrador del servidor.
2. En el **herramientas** menú, seleccione **Administrador de clústeres de conmutación por error**.
3. En el **Administrador de clústeres de conmutación por error** panel, en **administración**, seleccione **crear clúster**.
    
    Se abre el Asistente para crear clúster.
4. En el **antes de comenzar** página, seleccione **siguiente**.
5. Si el **seleccionar servidores** página aparece en el **escriba el nombre** cuadro, escriba el nombre NetBIOS o el nombre de dominio completo de un servidor que va a agregar como un nodo de clúster de conmutación por error y, a continuación, seleccione **Agregar**. Repite este paso para todos los servidores que quieras agregar. Para agregar varios servidores al mismo tiempo, separa los nombres con una coma o un punto y coma. Por ejemplo, escribe los nombres con el formato *server1.contoso.com; server2.contoso.com*. Cuando haya terminado, seleccione **siguiente**.
    
    >[!NOTE]
    >Si decide crear el clúster inmediatamente después de ejecutar la validación el [validar el procedimiento de configuración](#validate-the-configuration), no verá el **seleccionar servidores** página. Los nodos que se validaron se agregan automáticamente al Asistente para crear clúster, de modo que no tengas que introducirlos de nuevo.
6. Si antes omitiste la validación, aparecerá la página **Advertencia de validación** . Recomendamos encarecidamente ejecutar la validación de clústeres. Solo los clústeres que superan todas las pruebas de validación son compatibles con Microsoft. Para ejecutar las pruebas de validación, seleccione **Sí**y, a continuación, seleccione **siguiente**. Completar la configuración del Asistente para validar una como se describe en [validar la configuración](#validate-the-configuration).
7. En la página **Punto de acceso para administrar el clúster** , haz lo siguiente:
    
    1. En el cuadro **Nombre de clúster**, escribe el nombre que deseas utilizar para administrar el clúster. Antes de hacerlo, revisa la siguiente información:
        
          - Durante la creación del clúster, este nombre se registra como el objeto de equipo de clúster (también llamado *objeto de nombre de clúster* o *CNO*) en AD DS. Si especificas un nombre NetBIOS para el clúster, el CNO se crea en la misma ubicación en la que residen los objetos de equipo para los nodos de clúster. Puede ser el contenedor Equipos predeterminado o una OU.
          - Para especificar una ubicación diferente para el CNO, escribe el nombre distintivo de una OU en el cuadro **Nombre de clúster**. Por ejemplo: *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*.
          - Si un administrador de dominio ha preconfigurado el CNO en una OU diferente de donde residen los nodos de clúster, especifica el nombre distintivo que te proporcione el administrador de dominio.
    2. Si el servidor no tiene ningún adaptador de red configurado para usar DHCP, debes configurar una o varias direcciones IP estáticas para el clúster de conmutación por error. Selecciona la casilla situada junto a cada red que desees utilizar para la administración de clústeres. Seleccione el **dirección** campo junto a una red seleccionada y, a continuación, escriba la dirección IP que desea asignar al clúster. Esta dirección o direcciones IP se asociarán al nombre del clúster en el Sistema de nombres de dominio (DNS).
    3. Cuando haya terminado, seleccione **siguiente**.
8. En la página **Confirmación**, revisa la configuración. De manera predeterminada, la casilla **Agregar todo el almacenamiento apto al clúster** está seleccionada. Desactiva esta casilla si deseas realizar una de las siguientes acciones:
    
      - Deseas configurar el almacenamiento más adelante.
      - Tienes previsto crear espacios de almacenamiento en clúster mediante el Administrador de clústeres de conmutación por error o mediante los cmdlets de Windows PowerShell de clústeres de conmutación por error, y todavía no has creado espacios de almacenamiento en los Servicios de archivos y almacenamiento. Para obtener más información, consulte [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Seleccione **siguiente** para crear el clúster de conmutación por error.
10. En la página **Resumen**, confirma que el clúster de conmutación por error se ha creado correctamente. Si hubiera alguna advertencia o error, revisa los resultados del resumen o seleccione **Ver informe** para ver el informe completo. Seleccione **finalizar**.
11. Para confirmar que se ha creado el clúster, comprueba que su nombre aparece en el **Administrador de clústeres de conmutación por error**, en el árbol de navegación. Puede expandir el nombre del clúster y, a continuación, seleccione elementos bajo **nodos**, **almacenamiento** o **redes** para ver los recursos asociados.
    
    Ten en cuenta que el nombre del clúster podría tardar un poco en replicarse correctamente en DNS. Cuando el registro DNS correcta y la replicación, si selecciona **todos los servidores** en el administrador del servidor, el nombre del clúster debe aparecer como un servidor con una **manejabilidad** estado de **en línea** .

Una vez creado el clúster, puedes realizar acciones como comprobar la configuración de quórum del clúster y, opcionalmente, crear volúmenes compartidos de clúster (CSV). Para obtener más información, consulte [descripción quórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md) y [Use Cluster Shared Volumes en un clúster de conmutación por error](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Crear roles en clúster

Después de crear el clúster de conmutación por error, puedes crear roles en clúster para hospedar cargas de trabajo de clúster.

>[!NOTE]
>Para los roles en clúster que necesitan un punto de acceso de cliente, se crea un objeto de equipo virtual (VCO) en AD DS. De manera predeterminada, todos los VCO para el clúster se crean en el mismo contenedor o en la misma OU que el CNO. Ten en cuenta que, después de crear un clúster, puedes mover el CNO a cualquier OU.

Aquí le mostramos cómo crear un rol en clúster:

1. Utiliza el Administrador del servidor o Windows PowerShell para instalar el rol o la característica que sean necesarios para un rol en clúster en cada nodo de clúster de conmutación. Por ejemplo, si deseas crear un servidor de archivos en clúster, instala el rol de Servidor de archivos en todos los nodos de clúster.
    
    En la siguiente tabla se muestran los roles en clúster que puedes configurar en el Asistente para alta disponibilidad y la característica o el rol de servidor asociado que debes instalar como requisito previo.
    
    <table>
    <thead>
    <tr class="header">
    <th>Rol en clúster</th>
    <th>Requisito previo del rol o característica</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Servidor de espacios de nombres DFS</td>
    <td>Espacios de nombres DFS (parte del rol de Servidor de archivos)</td>
    </tr>
    <tr class="even">
    <td>Servidor DHCP</td>
    <td>Rol de Servidor DHCP</td>
    </tr>
    <tr class="odd">
    <td>Coordinador de transacciones distribuidas (DTC)</td>
    <td>Ninguno</td>
    </tr>
    <tr class="even">
    <td>Servidor de archivos</td>
    <td>Rol de Servidor de archivos</td>
    </tr>
    <tr class="odd">
    <td>Aplicación genérica</td>
    <td>No disponible</td>
    </tr>
    <tr class="even">
    <td>Script genérico</td>
    <td>No disponible</td>
    </tr>
    <tr class="odd">
    <td>Servicio genérico</td>
    <td>No disponible</td>
    </tr>
    <tr class="even">
    <td>Agente de réplicas de Hyper-V</td>
    <td>Rol de Hyper-V</td>
    </tr>
    <tr class="odd">
    <td>Servidor de destino iSCSI</td>
    <td>Servidor de destino iSCSI (parte del rol de Servidor de archivos)</td>
    </tr>
    <tr class="even">
    <td>Servidor iSNS</td>
    <td>Característica Servicio de servidor iSNS</td>
    </tr>
    <tr class="odd">
    <td>Message Queue Server</td>
    <td>Característica Servicios de Message Queue Server</td>
    </tr>
    <tr class="even">
    <td>Otro servidor</td>
    <td>Ninguno</td>
    </tr>
    <tr class="odd">
    <td>Máquina virtual</td>
    <td>Rol de Hyper-V</td>
    </tr>
    <tr class="even">
    <td>Servidor WINS</td>
    <td>Característica Servidor WINS</td>
    </tr>
    </tbody>
    </table>
2. En el Administrador de clústeres de conmutación por error, expanda el nombre del clúster, haga clic en **Roles**y, a continuación, seleccione **configurar rol**.
3. Sigue los pasos del Asistente para alta disponibilidad para crear el rol en clúster.
4. Para comprobar que se ha creado el rol en clúster, en el panel **Roles** , asegúrate de que el rol tenga el estado **En ejecución**. El panel Roles también indica el nodo propietario. Para probar la conmutación por error, haga clic en el rol, seleccione **mover**y, a continuación, seleccione **Seleccionar nodo**. En el **mover rol en clúster** cuadro de diálogo, seleccione el nodo de clúster deseado y, a continuación, seleccione **Aceptar**. En la columna **Nodo propietario** , comprueba que el nodo propietario ha cambiado.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Crear un clúster de conmutación por error a través de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizar las mismas funciones que los procedimientos anteriores de este tema. Escribe cada cmdlet en una sola línea, aunque aparezcan con salto entre varias líneas debido a las limitaciones de formato.

>[!NOTE]
>Debe usar Windows PowerShell para crear un clúster desconectado de Active Directory en Windows Server 2012 R2. Para obtener más información sobre la sintaxis, consulta [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

El siguiente ejemplo instala la característica Clústeres de conmutación por error.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

El siguiente ejemplo ejecuta todas las pruebas de validación de clústeres en los equipos llamados *Server1* y *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>El **Test-Cluster** cmdlet guarda el resultado en un archivo de registro en el directorio de trabajo actual. Por ejemplo: C:\Users\<username>\AppData\Local\Temp.

El siguiente ejemplo crea un clúster de conmutación por error llamado *MyCluster* con los nodos *Server1* y *Server2*, asigna la dirección IP estática *192.168.1.12*y agrega todo el almacenamiento apto al clúster de conmutación por error.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

El siguiente ejemplo crea el mismo clúster de conmutación por error que el ejemplo anterior, pero no agrega almacenamiento apto al clúster de conmutación por error.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

El siguiente ejemplo crea un clúster llamado *MyCluster* en la OU *Cluster* del dominio *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Para obtener ejemplos de cómo agregar roles en clúster, consulta temas como [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) y [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## <a name="more-information"></a>Más información

  - [Agrupación en clústeres de conmutación por error](failover-clustering.md)
  - [Implementar un clúster de Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Servidor de archivos de escalabilidad horizontal para datos de la aplicación](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Implementar un clúster desconectado de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Usar clústeres invitados para alta disponibilidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Actualización de clústeres](cluster-aware-updating.md)
  - [New-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Clúster de prueba](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
