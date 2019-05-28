---
title: Introducción a Windows Admin Center
description: Introducción a Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: ff1f949c764473a63eafa25346949d710699dbd1
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222721"
---
# <a name="get-started-with-windows-admin-center"></a>Introducción a Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows Admin Center instalados en Windows 10

> [!IMPORTANT]
> Debe ser miembro del grupo de administradores locales usen Windows Admin Center en Windows 10

### <a name="selecting-a-client-certificate"></a>Seleccionar un certificado de cliente

La primera vez que abra Windows Admin Center en Windows 10, asegúrese de seleccionar el *Windows Admin Center cliente* certificado (en caso contrario, obtendrá un error HTTP 403 que dice "no se puede obtener a esta página").

En Microsoft Edge, cuando se le pida con este cuadro de diálogo:
 
1. Haga clic en **más opciones**

    ![](../media/launch-cert-1.png)

2. Seleccione el certificado con la etiqueta **Windows Admin Center cliente** y haga clic en **Aceptar**

    ![](../media/launch-cert-2.png)

3. Asegúrese de que **permitir el acceso siempre** está seleccionada y haga clic en **permitir**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Conectarse a clústeres y nodos administrados

Después de haber completado la instalación de Windows Admin Center, puede agregar servidores o clústeres para administrar desde la página información general principal.

 **Agregar un solo servidor o un clúster como un nodo administrado**

 1. Haga clic en **+ agregar** en **todas las conexiones**.

    ![](../media/launch/addserver0.png)

 2. Elija esta opción Agregar una conexión de servidor, clúster de conmutación por error o un clúster de Hyper-Converged:
    
    ![](../media/launch/addserver1.png)

 3. Escriba el nombre del servidor o clúster para administrar y haga clic en **enviar**. El servidor o clúster se agregarán a la lista de conexiones en la página información general.

    ![](../media/launch/addserver2.png)

   **--O BIEN--**

**Una importación masiva de varios servidores**

 1. En el **Agregar conexión de servidor** página, elija el **importar servidores** ficha.

    ![](../media/launch/import-servers.png)

 2. Haga clic en **examinar** y seleccione un archivo de texto que contiene una coma o nueva línea separados, lista de FQDN para los servidores que desea agregar.

    **--O BIEN--**

**Agregar servidores de búsqueda en Active Directory**

 1. En el **Agregar conexión de servidor** página, elija el **buscar en Active Directory** ficha.

    ![](../media/launch/search-ad.png)

 2. Escriba los criterios de búsqueda y haga clic en **búsqueda**. Se admiten caracteres comodín (*).

 3. Una vez finalizada la búsqueda: seleccione uno o varios de los resultados, si lo desea agregar etiquetas y haga clic en **agregar**.

## <a name="authenticate-with-the-managed-node"></a>Autenticar con el nodo administrado ##

Windows Admin Center admite varios mecanismos para autenticar con un nodo administrado. Inicio de sesión único es el valor predeterminado.

**Inicio de sesión único**

Puede usar sus credenciales actuales de Windows para autenticarse con el nodo administrado. Este es el valor predeterminado, y Windows Admin Center trata el inicio de sesión cuando se agrega un servidor. 

**De sesión único cuando se implementa como un servicio de Windows Server**

Si ha instalado Windows Admin Center en Windows Server, configuración adicional es necesaria para el inicio de sesión único.  [Configurar el entorno para la delegación](..\configure\user-access-control.md)

**--O BIEN--**

**Use *administrar como* para especificar credenciales**

En **todas las conexiones**, seleccione un servidor de la lista y elija **administrar como** para especificar las credenciales que utilizará para autenticarse en el nodo administrado:

![](../media/launch-use-6.png)

Si se está ejecutando en modo de servicio de Windows Admin Center en Windows Server, pero no tiene configurada la delegación Kerberos, debe volver a escribir sus credenciales de Windows:

![](../media/launch-use-7.png)

Las credenciales se pueden aplicar a todas las conexiones, que se almacenará en memoria caché ellos para esa sesión de explorador específico. Si se vuelve a cargar el explorador, debe volver a escribir su **administrar como** credenciales.

**Solución de contraseña de administrador local (LAP)**

Si su entorno usa [LAPS](https://technet.microsoft.com/mt227395.aspx)y tiene Windows Admin Center instalados en los equipos Windows 10, puede usar las credenciales de LAPS para autenticarse con el nodo administrado. **Si usa este escenario, por favor,** [proporcionar comentarios](http://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Uso de etiquetas para organizar sus conexiones

Puede utilizar etiquetas para identificar y filtrar los servidores relacionados en la lista de conexiones.  Esto le permite ver un subconjunto de los servidores en la lista de conexiones.  Esto es especialmente útil si tiene muchas conexiones.

### <a name="edit-tags"></a>Editar etiquetas

* Seleccione un servidor o varios servidores en la lista de todas las conexiones
* En **todas las conexiones**, haga clic en **editar etiquetas**

![](../media/launch/tags-5.png)

El **editar etiquetas de conexión** panel le permite modificar, agregar o quitar etiquetas de sus conexiones seleccionadas:

* Para agregar una nueva etiqueta para sus conexiones seleccionadas, seleccione **Agregar etiqueta** y escriba el nombre de la etiqueta a la que le gustaría utilizar.

* Para etiquetar las conexiones seleccionadas con un nombre de etiqueta existente, active la casilla situada junto al nombre de etiqueta que desea aplicar.

* Para quitar una etiqueta de seleccionado todas las conexiones, desactive la casilla situada junto a la etiqueta a la que desea quitar.

* Si una etiqueta se aplica a un subconjunto de las conexiones seleccionadas, se muestra la casilla de verificación en un estado intermedio. Puede hacer clic en el cuadro para protegerlo y aplicar la etiqueta a todas las conexiones seleccionadas o hacer clic para desactivarla y quitar la etiqueta de todas las conexiones seleccionadas.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrar las conexiones por etiqueta

Una vez que se han agregado etiquetas a una o varias conexiones de servidor, puede ver las etiquetas en la lista de conexiones y filtrar la lista de conexiones por etiquetas.

* Para filtrar por una etiqueta, seleccione el icono de filtro situado junto al cuadro de búsqueda.
![](../media/launch/tags-7.png)
* Puede seleccionar "o", "y" o "no" para modificar el comportamiento de los filtros de las etiquetas seleccionadas.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usar PowerShell para importar o exportar las conexiones (con etiquetas)

> Se aplica a: Versión preliminar de Windows Admin Center

Versión preliminar de Windows Admin Center incluye un módulo de PowerShell para importar o exportar la lista de conexiones.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>Formato de archivo CSV para la importación de conexiones

El formato del archivo CSV se inicia con los encabezados de cuatro ```"name","type","tags","groupId"```, seguido por cada conexión en una nueva línea.

**nombre** es el FQDN de la conexión

**tipo** es el tipo de conexión. Para las conexiones de forma predeterminada incluidas con Windows Admin Center, usará uno de los siguientes:

| Tipo de conexión | Cadena de conexión |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| PC con Windows 10 | msft.sme.connection-type.windows-client |
| Clúster de conmutación por error | msft.sme.connection-type.cluster |
| Clúster Hiperconvergido | msft.sme.connection-type.hyper-converged-cluster |

**etiquetas** están separados mediante la canalización.

**groupId** se usa para las conexiones compartidas. Use el valor ```global``` en esta columna para hacer esto una conexión compartida.

### <a name="example-csv-file-for-importing-connections"></a>Archivo CSV de ejemplo para la importación de conexiones

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Importación de conexiones RDCman

Use el siguiente script para exportar las conexiones guardadas en [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) a un archivo. A continuación, puede importar el archivo en Windows Admin Center, para mantener la jerarquía de agrupación RDCMan mediante etiquetas. ¡Pruébelo!

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

2. Para crear una. Archivo CSV, ejecute el siguiente comando:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importar resultante. Archivo CSV en Windows Admin Center y toda la jerarquía de agrupación RDCMan se representará mediante etiquetas en la lista de conexiones. Para obtener más información, consulte [usar PowerShell para importar o exportar las conexiones (con etiquetas)](#use-powershell-to-import-or-export-your-connections-with-tags).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Ver los scripts de PowerShell usados en Windows Admin Center

Una vez que se ha conectado a un servidor, el clúster o el equipo, puede buscar en los scripts de PowerShell que impulsan las acciones de interfaz de usuario disponibles en Windows Admin Center. Desde una herramienta, haga clic en el icono de PowerShell en la barra de aplicaciones principales. Seleccione un comando de interés en la lista desplegable para desplazarse a la secuencia de comandos de PowerShell correspondiente.

![](../media/launch/showscript.png)