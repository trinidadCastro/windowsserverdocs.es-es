---
title: "Administrar las directivas de restricción de Software"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="administer-software-restriction-policies"></a>Administrar las directivas de restricción de Software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI contiene procedimientos cómo administrar las directivas de control de aplicación utilizar principio de directivas de restricción de Software (SRP) con Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Directivas de restricción de software (SRP) es la característica de directiva de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de esos programas para ejecutarse. Usa las directivas de restricción de software para crear una configuración muy restringida para equipos, en los que permites solo específicamente identificadas aplicaciones que se ejecutan. Estos se integran con Microsoft Active Directory Domain Services y la directiva de grupo, pero también pueden configurarse en equipos independientes. Para obtener más información acerca de SRP, consulta el [las directivas de restricción de Software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de, o conjuntamente con SRP para una parte de la estrategia de control de la aplicación.

Este tema contiene:

-   [Para abrir las directivas de restricción de Software](#BKMK_Open_SRP)

-   [Para crear nuevas directivas de restricción de software](#BKMK_Create_SRP)

-   [Para agregar o eliminar un tipo de archivo designados](#BKMK_Add_Del)

-   [Para impedir que las directivas de restricción de software se apliquen a los administradores locales](#BKMK_Prevent_Admin)

-   [Para cambiar el nivel de seguridad predeterminado de las directivas de restricción de software](#BKMK_Sec_Lvl)

-   [Para aplicar las directivas de restricción de software a DLL](#BKMK_Apply_SRP_DLLs)

Para obtener información acerca de cómo llevar a cabo tareas específicas con SRP, consulta los siguientes temas:

-   [Determinar permitir y denegar lista y un inventario de la aplicación para las directivas de restricción de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Trabajar con reglas de directivas de restricción de Software](work-with-software-restriction-policies-rules.md)

-   [Usar las directivas de restricción de Software para ayudar a proteger el equipo contra los Virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Para abrir las directivas de restricción de Software

-   [Para el equipo local](#BKMK_1)

-   [Para un dominio, sitio, o unidad organizativa y está en un servidor miembro o en una estación de trabajo que se ha unido a un dominio](#BKMK_2)

-   [Para un dominio o unidad organizativa y están en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto](#BKMK_3)

-   [Para un sitio, si está en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto](#BKMK_4)

### <a name="BKMK_1"></a>Para el equipo local

1.  Abre la configuración de seguridad Local.

2.  En el árbol de consola, haz clic en **las directivas de restricción de Software**.

    **¿Donde?**

    -   Directivas de restricción de Software y configuración de seguridad

> [!NOTE]
> Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente.

### <a name="BKMK_2"></a>Para un dominio, sitio, o unidad organizativa y está en un servidor miembro o en una estación de trabajo que se ha unido a un dominio

1.  Abre Microsoft Management Console (MMC).

2.  En la **archivo** menú, haz clic en **agregar o quitar complemento**y, a continuación, haz clic en **agregar**.

3.  Haz clic en **Editor de objetos de directiva de grupo Local**y, a continuación, haz clic en **agregar**.

4.  En **seleccionar un objeto de directiva de grupo**, haz clic en **examinar**.

5.  En **buscar un objeto de directiva de grupo**, selecciona un objeto de directiva de grupo (GPO) en el dominio, un sitio o una unidad organizativa- o crear uno nuevo y, a continuación, haz clic en **finalizar**.

6.  Haz clic en **cerrar**y, a continuación, haz clic en **Aceptar**.

7.  En el árbol de consola, haz clic en **las directivas de restricción de Software**.

    **¿Donde?**

    -   *Objeto de directiva de grupo* [*ComputerName*] / configuración del equipo o

        Directivas de restricción de Software o configuración de seguridad de usuario configuración de Windows

> [!NOTE]
> Para realizar este procedimiento, debe ser miembro del grupo Domain Admins.

### <a name="BKMK_3"></a>Para un dominio o unidad organizativa y están en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto

1.  Abre la consola de administración de directivas de grupo.

2.  En el árbol de consola, haz clic en el objeto de directiva de grupo (GPO) que quieres abrir las directivas de restricción de software para.

3.  Haz clic en **editar** para abrir el GPO que quieras editar. También puedes hacer clic en **nueva** para crear un nuevo GPO y, a continuación, haz clic en **editar**.

4.  En el árbol de consola, haz clic en **las directivas de restricción de Software**.

    **¿Donde?**

    -   *Objeto de directiva de grupo* [*ComputerName*] / configuración del equipo o

        Directivas de restricción de Software o configuración de seguridad de usuario configuración de Windows

> [!NOTE]
> Para realizar este procedimiento, debe ser miembro del grupo Domain Admins.

### <a name="BKMK_4"></a>Para un sitio, si está en un controlador de dominio o en una estación de trabajo que tenga instaladas las herramientas de administración de servidor remoto

1.  Abre la consola de administración de directivas de grupo.

2.  En el árbol de consola, haz clic en el sitio que quieras establecer la directiva de grupo para.

    **¿Donde?**

    -   Sitios de Active Directory y servicios [*nombreControladorDominio.NombreDominio*] / sitios/sitio

3.  Haz clic en una entrada en **vínculos de objeto de directiva de grupo** para seleccionar un objeto de directiva de grupo (GPO) existentes y, a continuación, haz clic en **editar**. También puedes hacer clic en **nueva** para crear un nuevo GPO y, a continuación, haz clic en **editar**.

4.  En el árbol de consola, haz clic en **las directivas de restricción de Software**.

    **Donde**

    -   *Objeto de directiva de grupo* [*ComputerName*] / configuración del equipo o

        Directivas de restricción de Software o configuración de seguridad de usuario configuración de Windows

> [!NOTE]
> -   Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio pueden realizar este procedimiento.
> -   Para establecer la configuración de directiva que se aplica a equipos, independientemente de que los usuarios inicien sesión en ellos, haz clic en **configuración del equipo**.
> -   Para establecer la configuración de directiva que se aplicará a los usuarios, independientemente del equipo que inicien sesión, haz clic en **configuración de usuario**.

## <a name="BKMK_Create_SRP"></a>Para crear nuevas directivas de restricción de software

1.  Abre las directivas de restricción de Software.

2.  En la **acción** menú, haz clic en **nuevas directivas de restricción de Software**.

> [!WARNING]
> -   Las credenciales administrativas son necesarias para realizar este procedimiento, en función del entorno:
> 
>     -   Si creas nuevas directivas de restricción de software para el equipo local: pertenencia local **administradores** grupo o equivalente, es lo mínimo necesario para completar este procedimiento.
>     -   Si creas nuevas directivas de restricción de software para un equipo que está unido a un dominio, los miembros del grupo Administradores de dominio pueden realizar este procedimiento.
> -   Si ya se ha creado para un objeto de directiva de grupo (GPO), a las directivas de restricción de software la **nuevas directivas de restricción de Software** comando no aparece en la **acción** menú. Para eliminar las directivas de restricción de software que se aplican a un GPO, en el árbol de consola, haz clic en **las directivas de restricción de Software**y, a continuación, haz clic en **eliminar las directivas de restricción de Software**. Cuando se eliminación las directivas de restricción de software para un GPO, también se eliminan todas las reglas de directivas de restricción de software para ese GPO. Después de eliminar las directivas de restricción de software, puedes crear nuevas directivas de restricción de software para ese GPO.

## <a name="BKMK_Add_Del"></a>Para agregar o eliminar un tipo de archivo designados

1.  Abre las directivas de restricción de Software.

2.  En el panel de detalles, haz doble clic en **tipos de archivo designados**.

3.  Realiza una de las siguientes acciones:

    -   Para agregar un tipo de archivo en **extensión de nombre de archivo**, escribe la extensión de nombre de archivo y, a continuación, haz clic en **agregar**.

    -   Para eliminar un tipo de archivo en **tipos de archivo designados**, haz clic en el tipo de archivo y, a continuación, haz clic en **quitar**.

> [!NOTE]
> -   Las credenciales administrativas son necesarias para realizar este procedimiento, según el entorno en el que agregar o eliminar un tipo de archivo designados:
> 
>     -   Si agrega o elimina un tipo de archivo designados para el equipo local: pertenencia local **administradores** grupo o equivalente, es lo mínimo necesario para completar este procedimiento.
>     -   Si creas nuevas directivas de restricción de software para un equipo que está unido a un dominio, los miembros del grupo Administradores de dominio pueden realizar este procedimiento.
> -   Puede ser necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   Todas las reglas para la configuración del equipo y configuración de usuario comparten la lista de tipos de archivo designados para un GPO.

## <a name="BKMK_Prevent_Admin"></a>Para impedir que las directivas de restricción de software se apliquen a los administradores locales

1.  Abre las directivas de restricción de Software.

2.  En el panel de detalles, haz doble clic en **aplicación**.

3.  En **las directivas de restricción de software se aplican a los usuarios siguientes**, haz clic en **todos los usuarios excepto los administradores locales**.

> [!WARNING]
> -   Pertenencia a locales **administradores** grupo o equivalente, es lo mínimo necesario para completar este procedimiento.
> -   Puede ser necesario crear una nueva configuración de directiva de restricción de software para el objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   Si es habitual que los usuarios sean miembros del grupo de administradores local en sus equipos de la organización, no puedes habilitar esta opción.
> -   Si vas a definir una configuración de directiva de restricción de software para el equipo local, usa este procedimiento para impedir que los administradores locales tengan directivas de restricción de software que se les aplicadas. Si vas a definir una configuración de directiva de restricción de software para la red, filtrar la configuración de directiva de usuario en función de pertenencia a grupos de seguridad a través de la directiva de grupo.

## <a name="BKMK_Sec_Lvl"></a>Para cambiar el nivel de seguridad predeterminado de las directivas de restricción de software

1.  Abre las directivas de restricción de Software.

2.  En el panel de detalles, haz doble clic en **niveles de seguridad**.

3.  Haz clic en el nivel de seguridad que quieras establecer como predeterminado y, a continuación, haz clic en **establecer como predeterminado**.

> [!CAUTION]
> En determinados directorios, establecer la configuración de seguridad predeterminada nivel a **no permitido** puede afectar negativamente al sistema operativo.

> [!NOTE]
> -   Las credenciales administrativas son necesarios para realizar este procedimiento, según el entorno para el que cambiar el nivel de seguridad predeterminado de las directivas de restricción de software.
> -   Puede ser necesario crear una nueva configuración de directiva de restricción de software para este objeto de directiva de grupo (GPO) si aún no lo has hecho.
> -   En el panel de detalles, el nivel de seguridad predeterminado actual está indicado por un círculo negro con una marca de verificación. Si haz clic en el nivel de seguridad actual de forma predeterminada, la **establecer como predeterminado** comando no aparece en el menú.
> -   Reglas de directivas de restricción de software se crean para especificar excepciones a nivel de seguridad predeterminado. Cuando se establece el nivel de seguridad de forma predeterminada en **Unrestricted**, las reglas pueden especificar software que no se puede ejecutar. Cuando se establece el nivel de seguridad de forma predeterminada en **no permitido**, las reglas pueden especificar software que se puede ejecutar.
> -   Durante la instalación, el nivel de seguridad predeterminado de las directivas de restricción de software en todos los archivos en el sistema se establece en **Unrestricted**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Para aplicar las directivas de restricción de software a DLL

1.  Abre las directivas de restricción de Software.

2.  En el panel de detalles, haz doble clic en **aplicación**.

3.  En **aplicar directivas de restricción de software al siguiente**, haz clic en **todos los archivos de software**.

> [!NOTE]
> -   Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio pueden realizar este procedimiento.
> -   De manera predeterminada, las directivas de restricción de software no comprueban bibliotecas de vínculos dinámicos (DLL). Comprobación de DLL puede reducir el rendimiento del sistema, porque las directivas de restricción de software deben evaluarse cada vez que se carga un archivo DLL. Sin embargo, puede comprobar los archivos DLL si le preocupa recibir un virus destinado a archivos DLL. Si se establece el nivel de seguridad de forma predeterminada en **no permitido**, y habilita la comprobación, debes crear las reglas de directivas que permitan cada DLL ejecutar restricción de software de DLL.


