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
ms.openlocfilehash: 5c0094c9cecfb50304b0317ab11c60f0332ef3a7
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518573"
---
# <a name="get-started-with-windows-admin-center"></a>Introducción al centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

> [!Tip]
> ¿No estás familiarizado con Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../overview.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Centro de administración de Windows instalado en Windows 10

> [!IMPORTANT]
> Debe ser miembro del grupo de administradores locales para usar el centro de administración de Windows en Windows 10.

### <a name="selecting-a-client-certificate"></a>Seleccionar un certificado de cliente

La primera vez que abra el centro de administración de Windows en Windows 10, asegúrese de seleccionar el certificado de *cliente del centro de administración de Windows* (de lo contrario, recibirá un error http 403 que indica "no se puede obtener acceso a esta página").

En Microsoft Edge, cuando se le pregunte con este cuadro de diálogo:

1. Haga clic en **más opciones**

    ![Seleccione un cuadro de certificado con más opciones resaltadas](../media/launch-cert-1.png)

2. Seleccione el certificado etiquetado **cliente del centro de administración de Windows** y haga clic en **Aceptar** .

    ![Selección de un cuadro de certificado que muestra los certificados disponibles](../media/launch-cert-2.png)

3. Asegúrese de que la opción **permitir el acceso siempre** está seleccionada y haga clic en **permitir** .

    ! Credencial requerida (cuadro de diálogo)] (.. launch-cert-3.png/media/)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Conectarse a clústeres y nodos administrados

Después de haber completado la instalación del centro de administración de Windows, puede agregar servidores o clústeres para administrarlos desde la página de información general principal.

 **Agregar un solo servidor o un clúster como nodo administrado**

1. Haga clic en **+ Agregar** en **Todas las conexiones**.

   ![Centro de administración de Windows: página todas las conexiones](../media/launch/addserver0.png)

2. Elija Agregar un servidor, un clúster, un equipo de Windows o una máquina virtual de Azure:

   ![Centro de administración de Windows: página Agregar recursos](../media/launch/ChooseConnectionType.png)

3. Escriba el nombre del servidor o clúster que desea administrar y haga clic en **submit (enviar**). El servidor o clúster se agregará a la lista de conexiones en la página información general.

   ![Centro de administración de Windows: Página servidores](../media/launch/addserver2.png)

   **--O bien--**

**Importación en bloque de varios servidores**

 1. En la página **Agregar conexión de servidor** , elija la pestaña **servidores de importación** .

    ![Centro de administración de Windows: pestaña servidores de importación](../media/launch/import-servers.png)

 2. Haga clic en **examinar** y seleccione un archivo de texto que contenga una coma o una nueva lista de FQDN para los servidores que desea agregar.

> [!Note]
> El archivo. csv que se crea al [exportar las conexiones con PowerShell](#use-powershell-to-import-or-export-your-connections-with-tags) contiene información adicional más allá de los nombres de servidor y no es compatible con este método de importación.

  **--O bien--**

**Agregar servidores buscando Active Directory**

 1. En la página **Agregar conexión de servidor** , elija la pestaña **Active Directory de búsqueda** .

    ![Centro de administración de Windows: pestaña buscar Active Directory](../media/launch/search-ad.png)

 2. Escriba los criterios de búsqueda y haga clic en **Buscar**. Se admiten caracteres comodín (*).

 3. Una vez completada la búsqueda, seleccione uno o varios de los resultados, agregue etiquetas de forma opcional y haga clic en **Agregar**.

## <a name="authenticate-with-the-managed-node"></a>Autenticación con el nodo administrado ##

El centro de administración de Windows admite varios mecanismos para la autenticación con un nodo administrado. El inicio de sesión único es el valor predeterminado.

**Inicio de sesión único**

Puede usar sus credenciales actuales de Windows para autenticarse con el nodo administrado. Este es el valor predeterminado y el centro de administración de Windows intenta iniciar sesión al agregar un servidor.

**Inicio de sesión único cuando se implementa como servicio en Windows Server**

Si ha instalado el centro de administración de Windows en Windows Server, se requiere una configuración adicional para el inicio de sesión único.  [Configurar el entorno para la delegación](../configure/user-access-control.md)

**--O bien--**

**Usar *administrar como* para especificar credenciales**

En **todas las conexiones**, seleccione un servidor de la lista y elija **administrar como** para especificar las credenciales que usará para autenticarse en el nodo administrado:

![Todas las conexiones, opción administrar como](../media/launch-use-6.png)

Si el centro de administración de Windows se ejecuta en modo de servicio en Windows Server, pero no tiene configurada la delegación Kerberos, debe volver a escribir las credenciales de Windows:

![Página especificar las credenciales](../media/launch-use-7.png)

Puede aplicar las credenciales a todas las conexiones, que se almacenarán en caché para esa sesión de explorador específica. Si recarga el explorador, debe volver a escribir las credenciales de **administrar como** .

**Solución de contraseña de administrador local (LAPS)**

Si su entorno usa [laps](https://technet.microsoft.com/mt227395.aspx)y tiene instalado el centro de administración de Windows en el equipo con Windows 10, puede usar las credenciales de laps para autenticarse con el nodo administrado. **Si usa este escenario,** indique [sus comentarios](https://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Uso de etiquetas para organizar las conexiones

Puede usar etiquetas para identificar y filtrar los servidores relacionados en la lista de conexiones.  Esto le permite ver un subconjunto de los servidores en la lista de conexiones.  Esto es especialmente útil si tiene muchas conexiones.

### <a name="edit-tags"></a>Editar etiquetas

* Seleccionar un servidor o varios servidores en la lista todas las conexiones
* En **todas las conexiones**, haga clic en **editar etiquetas** .

![Centro de administración de Windows: opción editar etiquetas](../media/launch/tags-5.png)

El panel **editar etiquetas de conexión** permite modificar, agregar o quitar etiquetas de las conexiones seleccionadas:

* Para agregar una nueva etiqueta a las conexiones seleccionadas, seleccione **Agregar etiqueta** y escriba el nombre de etiqueta que desea usar.

* Para etiquetar las conexiones seleccionadas con un nombre de etiqueta existente, active la casilla situada junto al nombre de etiqueta que desea aplicar.

* Para quitar una etiqueta de todas las conexiones seleccionadas, desactive la casilla situada junto a la etiqueta que desea quitar.

* Si se aplica una etiqueta a un subconjunto de las conexiones seleccionadas, la casilla se muestra en un estado intermedio. Puede hacer clic en el cuadro para comprobarlo y aplicar la etiqueta a todas las conexiones seleccionadas, o bien hacer clic de nuevo para desactivarla y quitar la etiqueta de todas las conexiones seleccionadas.

![Centro de administración de Windows: Página editar etiquetas de conexión](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrar conexiones por etiqueta

Una vez que se han agregado etiquetas a una o más conexiones de servidor, puede verlas en la lista de conexiones y filtrar la lista de conexiones por etiquetas.

* Para filtrar por una etiqueta, seleccione el icono de filtro situado junto al cuadro de búsqueda.

   ![Centro de administración de Windows: filtrar mediante el cuadro de búsqueda](../media/launch/tags-7.png)

   * Puede seleccionar "or", "and" o "not" para modificar el comportamiento de filtro de las etiquetas seleccionadas.

   ![Centro de administración de Windows: Página filtrar conexiones](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usar PowerShell para importar o exportar las conexiones (con etiquetas)

[!INCLUDE [ps-connections](../includes/ps-connections.md)]

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Ver scripts de PowerShell usados en el centro de administración de Windows

Una vez que se haya conectado a un servidor, un clúster o un equipo, puede consultar los scripts de PowerShell que alimentan las acciones de la interfaz de usuario disponibles en el centro de administración de Windows. En una herramienta, haga clic en el icono de PowerShell en la barra de la aplicación superior. Seleccione un comando de interés en la lista desplegable para navegar hasta el script de PowerShell correspondiente.

![Página ver scripts de PowerShell para información general](../media/launch/showscript.png)
