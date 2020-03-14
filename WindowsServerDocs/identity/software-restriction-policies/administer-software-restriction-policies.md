---
title: Administración de las directivas de restricción de software
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc22093-67d1-47b6-9ddd-4569b6761ce9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c75c7813041870f79ed95250857a5c7d1576c7dc
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322987"
---
# <a name="administer-software-restriction-policies"></a>Administración de las directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema para profesionales de ti contiene procedimientos para administrar directivas de control de aplicaciones mediante directivas de restricción de software (SRP) a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también se pueden configurar en equipos independientes. Para obtener más información acerca de SRP, consulte las [directivas de restricción de software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, se puede usar Windows AppLocker en lugar de o junto con SRP para una parte de la estrategia de control de la aplicación.

Este tema contiene:

-   [Para abrir directivas de restricción de software](#BKMK_Open_SRP)

-   [Para crear nuevas directivas de restricción de software](#BKMK_Create_SRP)

-   [Para agregar o eliminar un tipo de archivo designado](#BKMK_Add_Del)

-   [Para evitar que las directivas de restricción de software se apliquen a los administradores locales](#BKMK_Prevent_Admin)

-   [Para cambiar el nivel de seguridad predeterminado de las directivas de restricción de software](#BKMK_Sec_Lvl)

-   [Para aplicar directivas de restricción de software a archivos dll](#BKMK_Apply_SRP_DLLs)

Para obtener información acerca de cómo realizar tareas específicas con SRP, consulte lo siguiente:

-   [Determinar la lista de permitidos y el inventario de aplicaciones para las directivas de restricción de software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Trabajo con reglas de directivas de restricción de software](work-with-software-restriction-policies-rules.md)

-   [Usar directivas de restricción de software para ayudar a proteger el equipo contra un virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Para abrir directivas de restricción de software

-   [Para el equipo local](#BKMK_1)

-   [Para un dominio, un sitio o una unidad organizativa, y se encuentra en un servidor miembro o en una estación de trabajo que está unida a un dominio](#BKMK_2)

-   [Para un dominio o unidad organizativa, y está en un controlador de dominio o en una estación de trabajo que tiene instalado el Herramientas de administración remota del servidor](#BKMK_3)

-   [Para un sitio de y está en un controlador de dominio o en una estación de trabajo que tiene instalado el Herramientas de administración remota del servidor](#BKMK_4)

### <a name="BKMK_1"></a>Para el equipo local

1.  Abra Configuración de seguridad local

2.  En el árbol de consola, haga clic en **directivas de restricción de software**.

    **Mientras?**

    -   Configuración de seguridad/directivas de restricción de software

> [!NOTE]
> Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tienen que delegarle la autoridad adecuada.

### <a name="BKMK_2"></a>Para un dominio, un sitio o una unidad organizativa, y se encuentra en un servidor miembro o en una estación de trabajo que está unida a un dominio

1.  Abra Microsoft Management Console (MMC).

2.  En el menú **archivo** , haga clic en **Agregar o quitar complemento**y, a continuación, haga clic en **Agregar**.

3.  Haga clic en **Editor de directivas de grupo local** y, a continuación, haga clic en **Agregar**.

4.  En **Seleccionar un objeto de directiva de grupo**, haga clic en **Examinar**.

5.  En **Buscar un objeto de directiva de grupo**, seleccione un objeto de directiva de grupo (GPO) en el dominio, sitio o unidad organizativa correspondiente, o cree uno nuevo y, a continuación, haga clic en **Finalizar**.

6.  Haga clic en **Cerrar**y después, en **Aceptar**.

7.  En el árbol de consola, haga clic en **directivas de restricción de software**.

    **Mientras?**

    -   *Directiva de grupo objeto* [*NombreDeEquipo*] directiva/configuración del equipo o

        Configuración de usuario/configuración de Windows/configuración de seguridad/directivas de restricción de software

> [!NOTE]
> Para llevar a cabo este procedimiento, debe ser miembro del grupo Admins. del dominio.

### <a name="BKMK_3"></a>Para un dominio o unidad organizativa, y está en un controlador de dominio o en una estación de trabajo que tiene instalado el Herramientas de administración remota del servidor

1.  Abra Consola de administración de directivas de grupo.

2.  En el árbol de consola, haga clic con el botón secundario en el objeto de directiva de grupo (GPO) para el que desea abrir directivas de restricción de software.

3.  Haga clic en **Editar** para abrir el GPO que desee editar. También puede hacer clic en **Nuevo** para crear un nuevo GPO y, a continuación, haga clic en **Editar**.

4.  En el árbol de consola, haga clic en **directivas de restricción de software**.

    **Mientras?**

    -   *Directiva de grupo objeto* [*NombreDeEquipo*] directiva/configuración del equipo o

        Configuración de usuario/configuración de Windows/configuración de seguridad/directivas de restricción de software

> [!NOTE]
> Para llevar a cabo este procedimiento, debe ser miembro del grupo Admins. del dominio.

### <a name="BKMK_4"></a>Para un sitio de y está en un controlador de dominio o en una estación de trabajo que tiene instalado el Herramientas de administración remota del servidor

1.  Abra Consola de administración de directivas de grupo.

2.  En el árbol de consola, haga clic con el botón secundario en el sitio para el que desea establecer directiva de grupo.

    **Mientras?**

    -   Sitios y servicios de Active Directory [*Domain_Controller_Name. Domain_Name*]/sites/site

3.  Haga clic en una entrada de **Directiva de grupo vínculos de objeto** para seleccionar un objeto Directiva de grupo existente (GPO) y, a continuación, haga clic en **Editar**. También puede hacer clic en **Nuevo** para crear un nuevo GPO y, a continuación, haga clic en **Editar**.

4.  En el árbol de consola, haga clic en **directivas de restricción de software**.

    **Mientras**

    -   *Directiva de grupo objeto* [*NombreDeEquipo*] directiva/configuración del equipo o

        Configuración de usuario/configuración de Windows/configuración de seguridad/directivas de restricción de software

> [!NOTE]
> -   Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tienen que delegarle la autoridad adecuada. Si el equipo está unido a un dominio, los miembros del grupo Administradores del dominio podrían realizar este procedimiento.
> -   Para establecer la configuración de directiva que se aplicará a los equipos, independientemente de los usuarios que inicien sesión en ellos, haga clic en **configuración del equipo**.
> -   Para establecer la configuración de directiva que se aplicará a los usuarios, independientemente del equipo en el que inicien sesión, haga clic en **configuración de usuario**.

## <a name="BKMK_Create_SRP"></a>Para crear nuevas directivas de restricción de software

1.  Abra Directivas de restricción de software.

2.  En el menú **Acción**, haga clic en **Nuevas directivas de restricción de software**.

> [!WARNING]
> -   Dependiendo de su entorno, se requieren diferentes credenciales administrativas para realizar este procedimiento:
> 
>     -   Si crea nuevas directivas de restricción de software para el equipo local: la pertenencia al grupo local **administradores** , o equivalente, es lo mínimo necesario para completar este procedimiento.
>     -   Si crea nuevas directivas de restricción de software para un equipo unido a un dominio, los miembros del grupo de Administradores de dominio pueden llevar a cabo este procedimiento.
> -   Si ya se han creado directivas de restricción de software para un objeto de directiva de grupo (GPO), el comando **Nuevas directivas de restricción de software** no aparece en el menú **Acción**. Para eliminar las directivas de restricción de software aplicadas a un GPO, en el árbol de consola, haga clic con el botón secundario en **Directivas de restricción de software** y, luego, en **Eliminar directivas de restricción de software**. Al eliminar las directivas de restricción de software para un GPO, también se eliminan todas las reglas de directivas de restricción de software para ese GPO. Después de eliminar las directivas de restricción de software, puede crear nuevas directivas de restricción de software para ese GPO.

## <a name="BKMK_Add_Del"></a>Para agregar o eliminar un tipo de archivo designado

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Tipos de archivo designados**.

3.  Realice una de las siguientes acciones:

    -   Para agregar un tipo de archivo, en **Extensión de nombre de archivo**, escriba la extensión de nombre de archivo y, a continuación, haga clic en **Agregar**.

    -   Para eliminar un tipo de archivo, en **Tipos de archivo designados**, haga clic en el tipo de archivo y, a continuación, haga clic en **Quitar**.

> [!NOTE]
> -   Dependiendo del entorno en el que agregue o elimine un tipo de archivo designado, se requieren diferentes credenciales administrativas para realizar este procedimiento:
> 
>     -   Si agrega o elimina un tipo de archivo designado para el equipo local: la pertenencia al grupo local **administradores** , o equivalente, es lo mínimo necesario para completar este procedimiento.
>     -   Si crea nuevas directivas de restricción de software para un equipo unido a un dominio, los miembros del grupo de Administradores de dominio pueden llevar a cabo este procedimiento.
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Todas las reglas de configuración del equipo y configuración de usuario de un GPO comparten la lista de tipos de archivos designados.

## <a name="BKMK_Prevent_Admin"></a>Para evitar que las directivas de restricción de software se apliquen a los administradores locales

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Cumplimiento**.

3.  En **Aplicar directivas de restricción de software a los siguientes usuarios**, haga clic en **Todos los usuarios salvo administradores locales**.

> [!WARNING]
> -   La pertenencia al grupo local **Administradores**, o equivalente, es el requisito mínimo necesario para completar este procedimiento.
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Si es común que los usuarios sean miembros del grupo local de Administradores en los equipos de su organización, es posible que no quiera habilitar esta opción.
> -   Si define una configuración de directiva de restricción de software para el equipo local, use este procedimiento para impedir que las directivas de restricción de software se apliquen a los administradores locales. Si define una configuración de directiva de restricción de software para la red, filtre la configuración de directiva de usuario en función de la pertenencia a grupos de seguridad a través de directiva de grupo.

## <a name="BKMK_Sec_Lvl"></a>Para cambiar el nivel de seguridad predeterminado de las directivas de restricción de software

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Niveles de seguridad**.

3.  Haga clic con el botón secundario en el nivel de seguridad que quiera establecer como predeterminado y, a continuación, haga clic en **Establecer como predeterminado**.

> [!CAUTION]
> En algunos directorios, establecer el nivel de seguridad predeterminado como **No permitido** puede afectar negativamente a su sistema operativo.

> [!NOTE]
> -   Dependiendo del entorno para el que cambie el nivel de seguridad predeterminado de las directivas de restricción de software, se requieren diferentes credenciales administrativas para realizar este procedimiento:
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para este objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   En el panel de detalles, el nivel de seguridad predeterminado actual se indica con un círculo negro con una marca de verificación. Si hace clic con el botón secundario en el nivel de seguridad predeterminado actual, el comando **Establecer como predeterminado** no aparece en el menú.
> -   Se crean reglas de directivas de restricción de software para especificar las excepciones en el nivel de seguridad predeterminado. Cuando el nivel de seguridad predeterminado se establece en **Sin restricción**, las reglas pueden especificar el software que no tiene permiso para ejecutarse. Cuando el nivel de seguridad predeterminado se establece en **No permitido**, las reglas pueden especificar el software que tiene permiso para ejecutarse.
> -   Durante la instalación, el nivel de seguridad predeterminado de las directivas de restricción de software en todos los archivos del sistema se establece en **Sin restricción**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Para aplicar directivas de restricción de software a archivos dll

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Cumplimiento**.

3.  En **Aplicar directivas de restricción de software al siguiente**, haga clic en **Todos los archivos de software**.

> [!NOTE]
> -   Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tienen que delegarle la autoridad adecuada. Si el equipo está unido a un dominio, los miembros del grupo Administradores del dominio podrían realizar este procedimiento.
> -   De manera predeterminada, las directivas de restricción de software no comprueban las bibliotecas de vínculos dinámicos (DLL). Comprobar las DLL puede reducir el rendimiento del sistema, porque las directivas de restricción de software deben ser evaluadas cada vez que se carga una DLL. Sin embargo, puede elegir comprobar las DLL si le preocupa recibir un virus que ataque las DLL. Si el nivel de seguridad predeterminado se establece en no **permitido**y habilita la comprobación de dll, debe crear reglas de directivas de restricción de software que permitan la ejecución de cada dll.


