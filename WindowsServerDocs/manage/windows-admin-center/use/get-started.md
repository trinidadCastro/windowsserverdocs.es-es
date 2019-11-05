---
title: Introducción al centro de administración de Windows
description: Introducción al centro de administración de Windows
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 02/15/2019
ms.openlocfilehash: fac17cd5975eeb699f205888edbe3f1c30b43394
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73567148"
---
# <a name="get-started-with-windows-admin-center"></a>Introducción al centro de administración de Windows

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Centro de administración de Windows instalado en Windows 10

> [!IMPORTANT]
> Debe ser miembro del grupo de administradores locales para usar el centro de administración de Windows en Windows 10.

### <a name="selecting-a-client-certificate"></a>Seleccionar un certificado de cliente

La primera vez que abra el centro de administración de Windows en Windows 10, asegúrese de seleccionar el certificado de *cliente del centro de administración de Windows* (de lo contrario, recibirá un error http 403 que indica "no se puede obtener acceso a esta página").

En Microsoft Edge, cuando se le pregunte con este cuadro de diálogo:
 
1. Haga clic en **más opciones**

    ![](../media/launch-cert-1.png)

2. Seleccione el certificado etiquetado **cliente del centro de administración de Windows** y haga clic en **Aceptar** .

    ![](../media/launch-cert-2.png)

3. Asegúrese de que la opción **permitir el acceso siempre** está seleccionada y haga clic en **permitir** .

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Conectarse a clústeres y nodos administrados

Después de haber completado la instalación del centro de administración de Windows, puede agregar servidores o clústeres para administrarlos desde la página de información general principal.

 **Agregar un solo servidor o un clúster como nodo administrado**

1. Haga clic en **+ Agregar** en **todas las conexiones**.

   ![](../media/launch/addserver0.png)

2. Elija Agregar un servidor, un clúster, un equipo de Windows o una máquina virtual de Azure:
    
   ![](../media/launch/ChooseConnectionType.png)

3. Escriba el nombre del servidor o clúster que desea administrar y haga clic en **submit (enviar**). El servidor o clúster se agregará a la lista de conexiones en la página información general.

   ![](../media/launch/addserver2.png)

   **--O bien--**

**Importación en bloque de varios servidores**

 1. En la página **Agregar conexión de servidor** , elija la pestaña **servidores de importación** .

    ![](../media/launch/import-servers.png)

 2. Haga clic en **examinar** y seleccione un archivo de texto que contenga una coma o una nueva lista de FQDN para los servidores que desea agregar.

> [!Note]
> El archivo. csv que se crea al [exportar las conexiones con PowerShell](#use-powershell-to-import-or-export-your-connections-with-tags) contiene información adicional más allá de los nombres de servidor y no es compatible con este método de importación.

  **--O bien--**

**Agregar servidores buscando Active Directory**

 1. En la página **Agregar conexión de servidor** , elija la pestaña **Active Directory de búsqueda** .

    ![](../media/launch/search-ad.png)

 2. Escriba los criterios de búsqueda y haga clic en **Buscar**. Se admiten caracteres comodín (*).

 3. Una vez completada la búsqueda, seleccione uno o varios de los resultados, agregue etiquetas de forma opcional y haga clic en **Agregar**.

## <a name="authenticate-with-the-managed-node"></a>Autenticación con el nodo administrado ##

El centro de administración de Windows admite varios mecanismos para la autenticación con un nodo administrado. El inicio de sesión único es el valor predeterminado.

**Inicio de sesión único**

Puede usar sus credenciales actuales de Windows para autenticarse con el nodo administrado. Este es el valor predeterminado y el centro de administración de Windows intenta iniciar sesión al agregar un servidor. 

**Inicio de sesión único cuando se implementa como un servicio en Windows Server**

Si ha instalado el centro de administración de Windows en Windows Server, se requiere una configuración adicional para el inicio de sesión único.  [Configurar el entorno para la delegación](../configure/user-access-control.md)

**--O bien--**

**Usar *administrar como* para especificar credenciales**

En **todas las conexiones**, seleccione un servidor de la lista y elija **administrar como** para especificar las credenciales que usará para autenticarse en el nodo administrado:

![](../media/launch-use-6.png)

Si el centro de administración de Windows se ejecuta en modo de servicio en Windows Server, pero no tiene configurada la delegación Kerberos, debe volver a escribir las credenciales de Windows:

![](../media/launch-use-7.png)

Puede aplicar las credenciales a todas las conexiones, que se almacenarán en caché para esa sesión de explorador específica. Si recarga el explorador, debe volver a escribir las credenciales de **administrar como** .

**Solución de contraseña de administrador local (LAPS)**

Si su entorno usa [laps](https://technet.microsoft.com/mt227395.aspx)y tiene instalado el centro de administración de Windows en el equipo con Windows 10, puede usar las credenciales de laps para autenticarse con el nodo administrado. **Si usa este escenario,** indique [sus comentarios](https://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Uso de etiquetas para organizar las conexiones

Puede usar etiquetas para identificar y filtrar los servidores relacionados en la lista de conexiones.  Esto le permite ver un subconjunto de los servidores en la lista de conexiones.  Esto es especialmente útil si tiene muchas conexiones.

### <a name="edit-tags"></a>Editar etiquetas

* Seleccionar un servidor o varios servidores en la lista todas las conexiones
* En **todas las conexiones**, haga clic en **editar etiquetas** .

![](../media/launch/tags-5.png)

El panel **editar etiquetas de conexión** permite modificar, agregar o quitar etiquetas de las conexiones seleccionadas:

* Para agregar una nueva etiqueta a las conexiones seleccionadas, seleccione **Agregar etiqueta** y escriba el nombre de etiqueta que desea usar.

* Para etiquetar las conexiones seleccionadas con un nombre de etiqueta existente, active la casilla situada junto al nombre de etiqueta que desea aplicar.

* Para quitar una etiqueta de todas las conexiones seleccionadas, desactive la casilla situada junto a la etiqueta que desea quitar.

* Si se aplica una etiqueta a un subconjunto de las conexiones seleccionadas, la casilla se muestra en un estado intermedio. Puede hacer clic en el cuadro para comprobarlo y aplicar la etiqueta a todas las conexiones seleccionadas, o bien hacer clic de nuevo para desactivarla y quitar la etiqueta de todas las conexiones seleccionadas.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrar conexiones por etiqueta

Una vez que se han agregado etiquetas a una o más conexiones de servidor, puede verlas en la lista de conexiones y filtrar la lista de conexiones por etiquetas.

* Para filtrar por una etiqueta, seleccione el icono de filtro situado junto al cuadro de búsqueda.
![](../media/launch/tags-7.png)
* Puede seleccionar "or", "and" o "not" para modificar el comportamiento de filtro de las etiquetas seleccionadas.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usar PowerShell para importar o exportar las conexiones (con etiquetas)

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>Formato de archivo CSV para importar conexiones

El formato del archivo CSV comienza con los cuatro encabezados ```"name","type","tags","groupId"```, seguido de cada conexión en una línea nueva.

**Name** es el FQDN de la conexión.

el **tipo** es el tipo de conexión. En el caso de las conexiones predeterminadas incluidas con el centro de administración de Windows, utilizará una de las siguientes opciones:

| Tipo de conexión | Cadena de conexión |
|------|-------------------------------|
| Windows Server | msft. SME. Connection-Type. Server |
| EQUIPO Windows 10 | msft. SME. Connection-Type. Windows-Client |
| Clúster de conmutación por error | msft. SME. Connection-Type. Cluster |
| Clúster hiperconvergido | msft. SME. Connection-Type. hiperconvergid-Cluster |

las **etiquetas** se separan por canalización.

**GROUPID** se usa para las conexiones compartidas. Use el valor ```global``` en esta columna para que esta sea una conexión compartida.

### <a name="example-csv-file-for-importing-connections"></a>Archivo CSV de ejemplo para importar conexiones

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Importar conexiones RDCman

Use el script siguiente para exportar las conexiones guardadas en [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) a un archivo. Después, puede importar el archivo en el centro de administración de Windows y mantener la jerarquía de agrupación de RDCMan mediante etiquetas. Pruébelo.

1. Copie y pegue el código siguiente en la sesión de PowerShell:

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. Para crear un. Archivo CSV, ejecute el siguiente comando:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importe el resultante. En el centro de administración de Windows y todas las jerarquías de agrupación de RDCMan se representarán mediante etiquetas en la lista de conexiones. Para obtener más información, consulte [uso de PowerShell para importar o exportar las conexiones (con etiquetas)](#use-powershell-to-import-or-export-your-connections-with-tags).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Ver scripts de PowerShell usados en el centro de administración de Windows

Una vez que se haya conectado a un servidor, un clúster o un equipo, puede consultar los scripts de PowerShell que alimentan las acciones de la interfaz de usuario disponibles en el centro de administración de Windows. En una herramienta, haga clic en el icono de PowerShell en la barra de la aplicación superior. Seleccione un comando de interés en la lista desplegable para navegar hasta el script de PowerShell correspondiente.

![](../media/launch/showscript.png)
