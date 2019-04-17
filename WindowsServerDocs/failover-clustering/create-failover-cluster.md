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
ms.sourcegitcommit: 5cbaca9685720f11d896c4ca167c86e74c032feb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "6068983"
---
# Crear un clúster de conmutación por error

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

En este tema muestra cómo crear un clúster de conmutación por error con el complemento Administrador de clústeres de conmutación por error o Windows PowerShell. El tema centra en una implementación típica, donde se crean objetos de equipo para el clúster y sus asociados roles en clúster en los servicios de dominio de Active Directory (AD DS). Si vas a implementar un clúster de espacios de almacenamiento directo, en su lugar, consulta [Implementar espacios de almacenamiento directo](../storage/storage-spaces/deploy-storage-spaces-direct.md).

También puedes implementar un clúster desconectado de Active Directory. Este método de implementación te permite crear un clúster de conmutación por error sin permisos para crear objetos de equipo en AD DS o la necesidad de solicitar ese equipo objetos son preconfigurados en AD DS. Esta opción solo está disponible a través de Windows PowerShell y solo se recomienda para escenarios específicos. Para obtener más información, consulta [implementar un clúster de Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### Lista de comprobación: Crear un clúster de conmutación por error

|Estado|Tarea|Referencia|
|:---:|---|---|
|☐|Comprobar los requisitos previos|[Comprobar los requisitos previos](#verify-the-prerequisites)|
|☐|Instalar la característica de clústeres de conmutación por error en cada servidor que desea agregar como un nodo de clúster|[Instalar la característica de clústeres de conmutación por error](#install-the-failover-clustering-feature)|
|☐|Ejecutar el Asistente para la validación de clúster para validar la configuración|[Validar la configuración](#validate-the-configuration)|
|☐|Ejecutar el Asistente para crear clúster para crear el clúster de conmutación por error|[Crear el clúster de conmutación por error](#create-the-failover-cluster)|
|☐|Crear roles en clúster para las cargas de trabajo de clúster de host|[Crear roles en clúster](#create-clustered-roles)|

## Comprobar los requisitos previos

Antes de comenzar, comprueba los siguientes requisitos previos:

- Asegúrate de que todos los servidores que desea agregar como nodos de clúster se está ejecutando la misma versión de Windows Server.
- Revisar los requisitos de hardware para asegurarte de que la configuración es compatible. Para obtener más información, consulta los [requisitos de Hardware de clústeres de conmutación por error y opciones de almacenamiento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Si vas a crear un clúster de espacios de almacenamiento directo, vea los [requisitos de hardware de espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Para agregar el almacenamiento en clúster durante la creación del clúster, asegúrate de que todos los servidores pueden acceder al almacenamiento. (También puedes agregar almacenamiento en clúster después de crear el clúster.)
- Asegúrate de que todos los servidores que desea agregar como nodos del clúster estén unidos al mismo dominio de Active Directory.
- (Opcional) Crear una unidad organizativa (OU) y mover las cuentas de equipo de los servidores que desea agregar como nodos de clúster en la unidad organizativa. Como procedimiento recomendado, te recomendamos que coloques clústeres de conmutación por error en sus propias unidades Organizativas en AD DS. Esto puede ayudar a mayor control sobre qué configuración de directiva de grupo o la configuración de la plantilla de seguridad afectan a los nodos del clúster. Al aislar clústeres en su propia unidad organizativa, también ayuda a evitar la eliminación accidental de objetos de equipo de clúster.

Además, comprueba los siguientes requisitos de cuenta:

- Asegúrate de que la cuenta que quieras usar para crear el clúster es un usuario de dominio que tenga derechos de administrador en todos los servidores que desea agregar como nodos de clúster.
- Asegúrate de que una de las siguientes es verdadera:
    - El usuario que crea el clúster tiene el permiso de **objetos de equipo de crear** la unidad organizativa o el contenedor donde se encuentran los servidores que forman el clúster.
    - Si el usuario no tiene el permiso de **objetos de equipo de crear** , pida al administrador de dominio para preconfigurar un objeto de equipo de clúster para el clúster. Para obtener más información, consulta [Prestage clúster objetos de equipo en Active Directory Domain Services](prestage-cluster-adds.md).

>[!NOTE]
>Este requisito no se aplica si quieres crear un clúster desconectado de Active Directory en Windows Server 2012 R2. Para obtener más información, consulta [implementar un clúster de Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## Instalar la característica de clústeres de conmutación por error

Debes instalar la característica de clústeres de conmutación por error en cada servidor que desea agregar como un nodo de clúster de conmutación por error.

### Instalar la característica de clústeres de conmutación por error

1. Inicia el Administrador de servidores.
2. En el menú **Administrar** , selecciona **Agregar Roles y características**.
3. En la página **antes de comenzar** , seleccione **siguiente**.
4. En la página **Seleccionar el tipo de instalación** , seleccione **instalación basada en características o basado en roles**y, a continuación, seleccione **siguiente**.
5. En la página **Seleccionar servidor de destino** , selecciona el servidor donde quieres instalar la característica y, a continuación, seleccione **siguiente**.
6. En la página **Seleccionar roles de servidor** , seleccione **siguiente**.
7. En la página **Seleccionar características** , selecciona la casilla de verificación de **Clústeres de conmutación por error** .
8. Para instalar las herramientas de administración de clústeres de conmutación por error, selecciona **Agregar características**y, a continuación, seleccione **siguiente**.
9. En la página **Confirmar selecciones de instalación** , seleccione **instalar**.
<br>Un reinicio del servidor no es necesario para la característica de clústeres de conmutación por error.

10. Cuando se complete la instalación, seleccione **Cerrar**.
11. Repite este procedimiento en cada servidor que desea agregar como un nodo de clúster de conmutación por error.

>[!NOTE]
>Después de instalar la característica de clústeres de conmutación por error, te recomendamos que apliques las últimas actualizaciones de Windows Update. Además, para un clúster de conmutación por error basada en Windows Server 2012, revisa el artículo de Microsoft Support [clústeres recomendado revisiones y actualizaciones para conmutación por error basada en Windows Server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) e instalar todas las actualizaciones aplicables.

## Validar la configuración

Antes de crear el clúster de conmutación por error, te recomendamos encarecidamente que validar la configuración para asegurarte de que el hardware y la configuración de hardware es compatibles con clústeres de conmutación por error. Microsoft admite una solución de clúster solo si la configuración completa pasa la validación de todas las pruebas y si se certifica todo el hardware para la versión de Windows Server que ejecutan los nodos del clúster.

>[!NOTE]
>Debes tener al menos dos nodos para ejecutar todas las pruebas. Si tienes que solo un nodo, muchas de las pruebas de almacenamiento de información crítica no ejecutará.

### Ejecutar las pruebas de validación de clúster

1. En un equipo que tiene las herramientas de administración de clúster de conmutación por error instalados de herramientas de administración remota del servidor o en un servidor donde instalaste la característica de clústeres de conmutación por error, inicie el Administrador de clústeres de conmutación por error. Para hacer esto en un servidor, inicie el administrador del servidor y, a continuación, en el menú **Herramientas** , selecciona **El Administrador de clústeres de conmutación por error**.
2. En el panel del **Administrador de clústeres de conmutación por error** , debajo de **administración**, seleccione **Validar configuración**.
3. En la página **Antes de comenzar** , seleccione **siguiente**.
4. En la página **Seleccionar servidores o en un clúster** , en el cuadro **Escriba el nombre** , escribe el nombre NetBIOS o el nombre de dominio completo del servidor que vas a agregar como un nodo de clúster de conmutación por error y, a continuación, selecciona **Agregar**. Repite este paso para cada servidor que desea agregar. Para agregar varios servidores al mismo tiempo, separar los nombres por una coma o punto y coma. Por ejemplo, escribe los nombres en el formato *Servidor1.contoso.com, server2.contoso.com*. Cuando hayas terminado, seleccione **siguiente**.
5. En la página **Opciones de prueba** , selecciona **ejecutar todas las pruebas (recomendadas)** y, a continuación, seleccione **siguiente**.
6. En la página de **confirmación** , seleccione **siguiente**.

    La página de validación muestra el estado de las pruebas en ejecución.
7. En la página de **Resumen** , realice una de las siguientes acciones:
    
      - Si los resultados indican que las pruebas se completó correctamente y la configuración es ideal para clústeres y quieres crear el clúster inmediatamente, asegúrate de que se selecciona la casilla de verificación de **crear el clúster ahora mediante los nodos validados** y, a continuación, Seleccione **Finalizar**. A continuación, seguir el paso 4 del procedimiento de [crear el clúster de conmutación por error](#create-the-failover-cluster) .
      - Si los resultados indican que se produjeron errores o advertencias, selecciona el **Informe de vista** para ver los detalles y determinar qué problemas que deben corregirse. Se dan cuenta de que una advertencia para una prueba de validación particular indica que este aspecto del clúster de conmutación por error puede ser compatible, pero que posiblemente no cumpla los procedimientos recomendados.
        
        >[!NOTE]
        >Si recibes una advertencia para la prueba de validar la reserva persistente de espacios de almacenamiento de información, consulta la entrada de blog [Advertencia de validación de clúster de conmutación por error de Windows indica los discos que no son compatibles con las reservas para espacios de almacenamiento persistentes](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) para obtener más información.

Para obtener más información acerca de las pruebas de validación de hardware, consulta [Validar Hardware para un clúster de conmutación por error](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Crear el clúster de conmutación por error

Para completar este paso, asegúrate de que la cuenta de usuario que inicie sesión como cumple los requisitos que se describen en la sección de [comprobar los requisitos previos](#verify-the-prerequisites) de este tema.

1. Inicia el Administrador de servidores.
2. En el menú **Herramientas** , selecciona **El Administrador de clústeres de conmutación por error**.
3. En el panel del **Administrador de clústeres de conmutación por error** , debajo de **administración**, seleccione **Crear el clúster**.
    
    Abre el Asistente para creación de clúster.
4. En la página **Antes de comenzar** , seleccione **siguiente**.
5. Si aparece la página **Seleccionar servidores** , en el cuadro **Escriba el nombre** , escribe el nombre NetBIOS o el nombre de dominio completo del servidor que vas a agregar como un nodo de clúster de conmutación por error y, a continuación, selecciona **Agregar**. Repite este paso para cada servidor que desea agregar. Para agregar varios servidores al mismo tiempo, separar los nombres por una coma o punto y coma. Por ejemplo, escribe los nombres en el formato *Servidor1.contoso.com; server2.contoso.com*. Cuando hayas terminado, seleccione **siguiente**.
    
    >[!NOTE]
    >Si has elegido crear el clúster inmediatamente después de ejecutar la validación en el [procedimiento de configuración de validación](#validate-the-configuration), no verás la página **Seleccionar servidores** . Los nodos que se han validado se agregan automáticamente el Asistente para crear clúster para que no tienes que especificar de nuevo.
6. Si se omite la validación anteriormente, aparecerá la página de **Advertencia de validación** . Te recomendamos encarecidamente que ejecutan la validación de clúster. Solo los clústeres que pasan todas las pruebas de validación son compatibles con Microsoft. Para ejecutar las pruebas de validación, selecciona **"Sí"** y, a continuación, seleccione **siguiente**. Completar la configuración del Asistente para validar una como se describe en [validar la configuración](#validate-the-configuration).
7. En la página de **Punto de acceso para administrar el clúster** , hacer lo siguiente:
    
    1. En el cuadro de **Nombre de clúster** , escribe el nombre que quieres usar para administrar el clúster. Antes de que, revise la siguiente información:
        
          - Durante la creación del clúster, este nombre está registrado como el objeto de equipo de clúster (también conocida como el *objeto de nombre de clúster* o *CNO*) en AD DS. Si se especifica un nombre NetBIOS para el clúster, se crea el CNO en la misma ubicación donde se encuentran los objetos de equipo para los nodos del clúster. Esto puede ser el contenedor predeterminado equipos o una unidad organizativa.
          - Para especificar una ubicación diferente para el CNO, puedes escribir el nombre completo de una unidad organizativa en el cuadro de **Nombre de clúster** . Por ejemplo: *CN = nombreDeClúster, OU = clústeres, DC = Contoso, DC = com*.
          - Si un administrador de dominio ha preconfigurado el CNO en una unidad organizativa diferente que donde se encuentran los nodos del clúster, especifica el nombre completo que proporciona el Administrador de dominio.
    2. Si el servidor no tiene un adaptador de red que se configura para usar DHCP, debes configurar una o varias direcciones IP estáticas para el clúster de conmutación por error. Selecciona la casilla de verificación junto a cada red que quieras usar para la administración de clúster. Selecciona el campo de **dirección** junto a una red seleccionada y, a continuación, escribe la dirección IP que quieres asignar al clúster. Esta dirección IP (o direcciones) se asociará con el nombre del clúster en el sistema de nombres de dominio (DNS).
    3. Cuando hayas terminado, seleccione **siguiente**.
8. En la página de **confirmación** , revisa la configuración. De manera predeterminada, se selecciona la casilla de verificación **Agregar todo el almacenamiento apto para el clúster** . Desactiva esta casilla si quieres llevar a cabo cualquiera de las siguientes acciones:
    
      - Quieres configurar el almacenamiento más adelante.
      - Vas a crear espacios de almacenamiento en clúster mediante el Administrador de clústeres de conmutación por error o a través de los cmdlets de Windows PowerShell de clústeres de conmutación por error y aún no creaste espacios de almacenamiento en servicios de archivos y almacenamiento. Para obtener más información, consulta [Implementar espacios de almacenamiento de clúster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Seleccione **siguiente** para crear el clúster de conmutación por error.
10. En la página de **Resumen** , confirma que el clúster de conmutación por error se creó correctamente. Si se han producido advertencias o errores, ver el resumen de resultados o seleccionar **Ver el informe** para ver el informe completo. Seleccione **Finalizar**.
11. Para confirmar que se ha creado el clúster, comprueba que el nombre del clúster aparece en **El Administrador de clústeres de conmutación por error** en el árbol de navegación. Puedes expandir el nombre del clúster y, a continuación, seleccionar elementos en **los nodos**, **almacenamiento** o **redes** para ver los recursos asociados.
    
    Se dan cuenta que puede tardar algún tiempo para el nombre del clúster replicar correctamente en DNS. Después de registro DNS se realiza correctamente y la replicación, si seleccionas **Todos los servidores** en el Administrador de servidores, el nombre del clúster debe mostrarse como un servidor con un estado de **facilidad de uso** de **en línea**.

Después de crea el clúster, puedes hacer cosas como comprobar la configuración de cuórum de clúster y, opcionalmente, crear volúmenes compartidos de clúster (CSV). Para obtener más información, consulta [Cuórum de descripción en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md) y [Volúmenes compartidos de clúster usa en un clúster de conmutación por error](failover-cluster-csvs.md).

## Crear roles en clúster

Después de crear el clúster de conmutación por error, puedes crear roles en clúster para las cargas de trabajo de clúster de host.

>[!NOTE]
>Para los roles en clúster que requieren un punto de acceso de cliente, se crea un objeto de equipo virtual (VCO) en AD DS. De manera predeterminada, todos los VCO para el clúster se crean en la misma unidad organizativa o contenedor como el CNO. Se dan cuenta que después de crear un clúster, puede mover el CNO a cualquier unidad organizativa.

Aquí te mostramos cómo crear un rol en el clúster:

1. El administrador del servidor de uso o Windows PowerShell para instalar el rol o característica que es necesaria para un rol en el clúster en cada nodo del clúster de conmutación por error. Por ejemplo, si quieres crear un servidor de archivos agrupados en clúster, instala el rol de servidor de archivos en todos los nodos del clúster.
    
    La siguiente tabla muestra los roles en clúster que puedes configurar en el Asistente para alta disponibilidad y el rol de servidor asociado o característica que debe instalar como requisito previo.
    
    <table>
    <thead>
    <tr class="header">
    <th>Rol en el clúster</th>
    <th>Rol o característica previo</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Servidor de Namespace DFS</td>
    <td>Espacios de nombres DFS (parte del rol de servidor de archivos)</td>
    </tr>
    <tr class="even">
    <td>Servidor DHCP</td>
    <td>Rol de servidor DHCP</td>
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
    <td>No aplicable</td>
    </tr>
    <tr class="even">
    <td>Secuencia de comandos genérica</td>
    <td>No aplicable</td>
    </tr>
    <tr class="odd">
    <td>Servicio genérico</td>
    <td>No aplicable</td>
    </tr>
    <tr class="even">
    <td>Agente de réplica de Hyper-V</td>
    <td>Rol de Hyper-V</td>
    </tr>
    <tr class="odd">
    <td>Servidor de destino iSCSI</td>
    <td>(parte del rol de servidor de archivos) del servidor de destino iSCSI</td>
    </tr>
    <tr class="even">
    <td>Servidor iSNS</td>
    <td>característica del servicio de servidor iSNS</td>
    </tr>
    <tr class="odd">
    <td>Message Queuing</td>
    <td>Característica de servicios de la cola de mensajes</td>
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
    <td>Característica de servidor WINS</td>
    </tr>
    </tbody>
    </table>
2. En el Administrador de clústeres de conmutación por error, expande el nombre del clúster, haz clic en **Roles**y, a continuación, selecciona el **Rol de configurar**.
3. Sigue los pasos descritos en el Asistente para alta disponibilidad para crear el rol en el clúster.
4. Para comprobar que se creó el rol en el clúster, en el panel de **Roles** , asegúrate de que la función tiene un estado de **ejecución**. El panel de Roles también indica el nodo propietario. Para probar la conmutación por error, haz clic en el rol, apuntar a **mover**y, a continuación, selecciona **Seleccionar nodo**. En el cuadro de diálogo **Mover función agrupado** , selecciona el nodo del clúster deseado y, a continuación, selecciona **Aceptar**. En la columna de **Nodo propietario** , comprueba que el nodo propietario se ha modificado.

## Crear un clúster de conmutación por error mediante Windows PowerShell

Los siguientes cmdlets de Windows PowerShell, realizan las mismas funciones como los procedimientos anteriores de este tema. Escribe cada cmdlet en una sola línea, aunque pueden aparecer con ajuste a través de varias líneas debido a restricciones de formato.

>[!NOTE]
>Debes usar Windows PowerShell para crear un clúster desconectado de Active Directory en Windows Server 2012 R2. Para obtener información sobre la sintaxis, consulta [implementar un clúster de Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

En el ejemplo siguiente se instala la característica de clústeres de conmutación por error.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

El siguiente ejemplo ejecuta todas las pruebas de validación de clúster en los equipos que se denominan *servidor 1* y el *servidor 2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>El cmdlet **Test-Cluster** proporciona los resultados en un archivo de registro en el directorio de trabajo actual. Por ejemplo: \AppData\Local\Temp C:\Users\ < nombre de usuario >.

El siguiente ejemplo crea un clúster de conmutación por error que se denomina *miClúster* con nodos de *servidor 1* y el *servidor 2*, asigna la dirección IP estática *192.168.1.12*y se agrega todo el almacenamiento apto para el clúster de conmutación por error.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

El siguiente ejemplo crea el mismo clúster de conmutación por error como en el ejemplo anterior, pero no agrega almacenamiento apto para el clúster de conmutación por error.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

El siguiente ejemplo crea un clúster denominado *miClúster* del *clúster* OU del dominio *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Para obtener ejemplos sobre cómo agregar roles en clúster, consulta los temas como [Agregar ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) y [Agregar ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## Más información

  - [Conmutación de clústeres por error](failover-clustering.md)
  - [Implementar un clúster de Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Servidor de archivos de escalabilidad horizontal para datos de la aplicación](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Implementar un clúster desconectado de directorio activo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Usar clústeres invitados para alta disponibilidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Actualización compatible con clústeres](cluster-aware-updating.md)
  - [Nuevo clúster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
