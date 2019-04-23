---
title: Administración de las directivas de restricción de software
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 67ef99c95f10585ab72dee4bdf6512e715d13f4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863596"
---
# <a name="administer-software-restriction-policies"></a>Administración de las directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI contiene procedimientos administrar directivas de control de aplicaciones con directivas de restricción de Software (SRP) que comience con Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también puede configurarse en equipos independientes. Para obtener más información sobre SRP, consulte el [Software Restriction Policies](software-restriction-policies.md).

Empezando por Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de o junto con SRP para una parte de su estrategia de control de la aplicación.

Este tema contiene:

-   [Para abrir las directivas de restricción de Software](#BKMK_Open_SRP)

-   [Para crear nuevas directivas de restricción de software](#BKMK_Create_SRP)

-   [Para agregar o eliminar un tipo de archivo designado](#BKMK_Add_Del)

-   [Para impedir que los administradores locales apliquen las directivas de restricción de software](#BKMK_Prevent_Admin)

-   [Para cambiar el nivel de seguridad predeterminada de las directivas de restricción de software](#BKMK_Sec_Lvl)

-   [Para aplicar las directivas de restricción de software a los archivos DLL](#BKMK_Apply_SRP_DLLs)

Para obtener información acerca de cómo realizar tareas específicas con SRP, consulte lo siguiente:

-   [Determinar la lista denegaciones y del inventario de aplicaciones para las directivas de restricción de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Trabajar con reglas de directivas de restricción de Software](work-with-software-restriction-policies-rules.md)

-   [Utilizar directivas de restricción de Software para ayudar a proteger su equipo contra Virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Para abrir las directivas de restricción de Software

-   [Para el equipo local](#BKMK_1)

-   [Para un dominio, sitio o unidad organizativa y está en un servidor miembro o una estación de trabajo que se ha unido a un dominio](#BKMK_2)

-   [Para un dominio o unidad organizativa y está en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto](#BKMK_3)

-   [Para un sitio, si está en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto](#BKMK_4)

### <a name="BKMK_1"></a>Para el equipo local

1.  Abra Configuración de seguridad local

2.  En el árbol de consola, haga clic en **Software Restriction Policies**.

    **¿Dónde?**

    -   Directivas de restricción de Software/configuración de seguridad

> [!NOTE]
> Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente.

### <a name="BKMK_2"></a>Para un dominio, sitio o unidad organizativa y está en un servidor miembro o una estación de trabajo que se ha unido a un dominio

1.  Abra Microsoft Management Console (MMC).

2.  En el **archivo** menú, haga clic en **agregar o quitar complemento**y, a continuación, haga clic en **agregar**.

3.  Haga clic en **Editor de directivas de grupo local** y, a continuación, haga clic en **Agregar**.

4.  En **Seleccionar un objeto de directiva de grupo**, haga clic en **Examinar**.

5.  En **buscar un objeto de directiva de grupo**, seleccione un objeto de directiva de grupo (GPO) en el dominio adecuado, sitio o unidad organizativa- o cree uno nuevo y, a continuación, haga clic en **finalizar**.

6.  Haga clic en **Cerrar**y después, en **Aceptar**.

7.  En el árbol de consola, haga clic en **Software Restriction Policies**.

    **¿Dónde?**

    -   *Objeto de directiva de grupo* [*ComputerName*] directiva/configuración del equipo o

        Directivas de restricción de configuración o Software de usuario Windows/Configuración de seguridad

> [!NOTE]
> Para llevar a cabo este procedimiento, debe ser miembro del grupo Admins. del dominio.

### <a name="BKMK_3"></a>Para un dominio o unidad organizativa y está en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto

1.  Abra la consola de administración de directivas de grupo.

2.  En el árbol de consola, haga clic en el objeto de directiva de grupo (GPO) que desea abrir directivas de restricción de software para.

3.  Haga clic en **Editar** para abrir el GPO que desee editar. También puede hacer clic en **Nuevo** para crear un nuevo GPO y, a continuación, haga clic en **Editar**.

4.  En el árbol de consola, haga clic en **Software Restriction Policies**.

    **¿Dónde?**

    -   *Objeto de directiva de grupo* [*ComputerName*] directiva/configuración del equipo o

        Directivas de restricción de configuración o Software de usuario Windows/Configuración de seguridad

> [!NOTE]
> Para llevar a cabo este procedimiento, debe ser miembro del grupo Admins. del dominio.

### <a name="BKMK_4"></a>Para un sitio, si está en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto

1.  Abra la consola de administración de directivas de grupo.

2.  En el árbol de consola, haga clic en el sitio que desea establecer la directiva de grupo para.

    **¿Dónde?**

    -   Servicios y sitios de Active Directory [*nombreControladorDominio.NombreDominio*] / Sites/sitio

3.  Haga clic en una entrada **vínculos de objeto de directiva de grupo** para seleccionar un objeto de directiva de grupo (GPO) existente y, a continuación, haga clic en **editar**. También puede hacer clic en **Nuevo** para crear un nuevo GPO y, a continuación, haga clic en **Editar**.

4.  En el árbol de consola, haga clic en **Software Restriction Policies**.

    **Where**

    -   *Objeto de directiva de grupo* [*ComputerName*] directiva/configuración del equipo o

        Directivas de restricción de configuración o Software de usuario Windows/Configuración de seguridad

> [!NOTE]
> -   Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento.
> -   Para establecer la configuración de directiva que se aplicará a los equipos, independientemente de que los usuarios inicien sesión en ellos, haga clic en **configuración del equipo**.
> -   Para establecer la configuración de directiva que se aplicará a los usuarios, independientemente de qué equipo inicien sesión, haga clic en **configuración de usuario**.

## <a name="BKMK_Create_SRP"></a>Para crear nuevas directivas de restricción de software

1.  Abra Directivas de restricción de software.

2.  En el menú **Acción**, haga clic en **Nuevas directivas de restricción de software**.

> [!WARNING]
> -   Dependiendo de su entorno, se requieren diferentes credenciales administrativas para realizar este procedimiento:
> 
>     -   Si crea nuevas directivas de restricción de software para el equipo local: Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.
>     -   Si crea nuevas directivas de restricción de software para un equipo unido a un dominio, los miembros del grupo de Administradores de dominio pueden llevar a cabo este procedimiento.
> -   Si ya se han creado directivas de restricción de software para un objeto de directiva de grupo (GPO), el comando **Nuevas directivas de restricción de software** no aparece en el menú **Acción**. Para eliminar las directivas de restricción de software aplicadas a un GPO, en el árbol de consola, haga clic con el botón secundario en **Directivas de restricción de software** y, luego, en **Eliminar directivas de restricción de software**. Al eliminar las directivas de restricción de software para un GPO, también elimina todas las reglas de directivas de restricción de software para ese GPO. Después de eliminar las directivas de restricción de software, puede crear nuevas directivas de restricción de software para ese GPO.

## <a name="BKMK_Add_Del"></a>Para agregar o eliminar un tipo de archivo designado

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Tipos de archivo designados**.

3.  Realiza una de las siguientes acciones:

    -   Para agregar un tipo de archivo, en **Extensión de nombre de archivo**, escriba la extensión de nombre de archivo y, a continuación, haga clic en **Agregar**.

    -   Para eliminar un tipo de archivo, en **Tipos de archivo designados**, haga clic en el tipo de archivo y, a continuación, haga clic en **Quitar**.

> [!NOTE]
> -   Dependiendo del entorno en el que agregue o elimine un tipo de archivo designado, se requieren diferentes credenciales administrativas para realizar este procedimiento:
> 
>     -   Si agrega o elimina un tipo de archivo designado en el equipo local: Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.
>     -   Si crea nuevas directivas de restricción de software para un equipo unido a un dominio, los miembros del grupo de Administradores de dominio pueden llevar a cabo este procedimiento.
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Todas las reglas comparten la lista de tipos de archivo designados para la configuración del equipo y configuración de usuario para un GPO.

## <a name="BKMK_Prevent_Admin"></a>Para impedir que los administradores locales apliquen las directivas de restricción de software

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Cumplimiento**.

3.  En **Aplicar directivas de restricción de software a los siguientes usuarios**, haga clic en **Todos los usuarios salvo administradores locales**.

> [!WARNING]
> -   Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   Si es común que los usuarios sean miembros del grupo local de Administradores en los equipos de su organización, es posible que no quiera habilitar esta opción.
> -   Si define una configuración de directiva de restricción de software para el equipo local, use este procedimiento para impedir que las directivas de restricción de software se apliquen a los administradores locales. Si va a definir una configuración de directiva de restricción de software para la red, filtrar según la pertenencia a grupos de seguridad mediante la directiva de grupo de configuración de directiva de usuario.

## <a name="BKMK_Sec_Lvl"></a>Para cambiar el nivel de seguridad predeterminada de las directivas de restricción de software

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Niveles de seguridad**.

3.  Haga clic con el botón secundario en el nivel de seguridad que quiera establecer como predeterminado y, a continuación, haga clic en **Establecer como predeterminado**.

> [!CAUTION]
> En algunos directorios, establecer el nivel de seguridad predeterminado como **No permitido** puede afectar negativamente a su sistema operativo.

> [!NOTE]
> -   Dependiendo del entorno para el que cambie el nivel de seguridad predeterminado de las directivas de restricción de software, se requieren diferentes credenciales administrativas para realizar este procedimiento:
> -   Es posible que sea necesario crear una nueva configuración de directiva de restricción de software para este objeto de directiva de grupo (GPO), si aun no lo ha hecho.
> -   En el panel de detalles, el nivel de seguridad predeterminado actual se indica con un círculo negro con una marca de verificación. Si hace clic con el botón secundario en el nivel de seguridad predeterminado actual, el comando **Establecer como predeterminado** no aparece en el menú.
> -   Para especificar las excepciones al nivel de seguridad de forma predeterminada, se crean reglas de directivas de restricción de software. Cuando el nivel de seguridad predeterminado se establece en **Sin restricción**, las reglas pueden especificar el software que no tiene permiso para ejecutarse. Cuando el nivel de seguridad predeterminado se establece en **No permitido**, las reglas pueden especificar el software que tiene permiso para ejecutarse.
> -   Durante la instalación, el nivel de seguridad predeterminado de las directivas de restricción de software en todos los archivos del sistema se establece en **Sin restricción**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Para aplicar las directivas de restricción de software a los archivos DLL

1.  Abra Directivas de restricción de software.

2.  En el panel de detalles, haga doble clic en **Cumplimiento**.

3.  En **Aplicar directivas de restricción de software al siguiente**, haga clic en **Todos los archivos de software**.

> [!NOTE]
> -   Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento.
> -   De manera predeterminada, las directivas de restricción de software no comprueban las bibliotecas de vínculos dinámicos (DLL). Comprobar las DLL puede reducir el rendimiento del sistema, porque las directivas de restricción de software deben ser evaluadas cada vez que se carga una DLL. Sin embargo, puede elegir comprobar las DLL si le preocupa recibir un virus que ataque las DLL. Si el nivel de seguridad predeterminado se establece en **no permitido**, y habilita la comprobación de DLL, debe crear reglas de directivas que permiten que cada DLL ejecutar restricción de software.


