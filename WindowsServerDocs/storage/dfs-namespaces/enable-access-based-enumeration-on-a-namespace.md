---
title: "Habilitar la enumeración basada en el acceso en un espacio de nombres"
description: "En este artículo se describe cómo habilitar la enumeración basada en el acceso en un espacio de nombres."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8c7ff3bde0a88c7c64cb5b1b6e656adab0e14452
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>Habilitar la enumeración basada en el acceso en un espacio de nombres

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

La enumeración basada en el acceso oculta los archivos y las carpetas para los que los usuarios no tienen permiso de acceso. De forma predeterminad, esta característica no está habilitada para los espacios de nombres DFS. Puedes habilitar la enumeración basada en el acceso de carpetas DFS mediante la opción de administración de DFS. Para controlar la enumeración basada en el acceso de archivos y carpetas en destinos de carpeta, debes habilitar la enumeración basada en el acceso en cada carpeta compartida mediante el uso compartido y la administración del almacenamiento.

Para habilitar la enumeración basada en el acceso en un espacio de nombres, todos los servidores de espacio de nombres deben ejecutar Windows Server 2008 o posterior. Además, los espacios de nombres de dominio deben usar el modo Windows Server 2008. Para obtener información sobre los requisitos del modo Windows Server 2008, consulta [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md).

En algunos entornos, la habilitación de la enumeración basada en el acceso puede generar un uso elevado de la CPU en el servidor y tiempos de respuesta demasiado elevados para los usuarios.

> [!NOTE]
> Si actualizas el nivel funcional de dominio a Windows Server 2008 y ya existen espacios de nombres basados en dominio, la opción de administración de DFS te permitirá habilitar la enumeración basada en el acceso en estos espacios de nombres. Sin embargo, no podrás editar permisos para ocultar las carpetas de los grupos o usuarios a no ser que migres los espacios de nombres al modo Windows Server 2008. Para obtener más información, consulta [Migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).


Para usar la enumeración basada en el acceso con espacios de nombres DFS, debes seguir estos pasos:

-   Habilitar la enumeración basada en el acceso en un espacio de nombres
-   Controlar los usuarios y grupos que pueden ver las carpetas individuales de DFS


> [!WARNING]
> La enumeración basada en el acceso no impide que los usuarios obtengan una referencia a un destino de carpeta si ya conocen la ruta de acceso DFS. Solo los permisos de recursos compartidos o los permisos del sistema de archivos NTFS del destino de carpeta (carpeta compartida) pueden evitar que los usuarios tengan acceso a un destino de carpeta. Permisos de las carpetas DFS solo se usan para mostrar u ocultar las carpetas DFS, no para controlar el acceso, acceso de lectura es el permiso solo es relevante en el nivel de carpeta DFS. Para obtener más información, consulta [Usar permisos heredados con la enumeración basada en el acceso](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx)

<br />
Puedes Habilitar la enumeración basada en el acceso en un espacio de nombres mediante el uso de la interfaz de Windows o una línea de comandos.

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>Para habilitar la enumeración basada en el acceso mediante la interfaz de Windows

1.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en el espacio de nombres correspondiente y luego haz clic en **Propiedades**.

2.  Haz clic en la pestaña **Opciones avanzadas** y luego selecciona la casilla de verificación **Habilitar enumeración basada en el acceso de este espacio de nombres**.

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>Para habilitar la enumeración basada en el acceso mediante una línea de comandos

1.  Abre una ventana del símbolo del sistema en un servidor que tenga el servicio de rol **Sistema de archivos distribuido** o la característica **Herramientas del sistema de archivos distribuido**.

2.  Escribe el siguiente comando, donde *<namespace\_root>* es la raíz del espacio de nombres:

    ```  
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> Para administrar la enumeración basada en el acceso en un espacio de nombres mediante Windows PowerShell, usa los cmdlets [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx), [Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx) y [Revoke-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx). El módulo de WindowsPowerShell para DFSN se introdujo en WindowsServer2012.

Puedes controlar los usuarios y grupos que pueden ver carpetas DFS individuales mediante el uso de la interfaz de Windows o una línea de comandos.

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>Para controlar la visibilidad de carpeta mediante la interfaz de Windows

1.  En el árbol de consola, en el nodo **Espacios de nombres**, busca la carpeta con destinos para la que deseas controlar la visibilidad, haz clic en ella y luego haz clic en **Propiedades**.

2.  Haz clic en la pestaña **Opciones avanzadas**.

3.  Haz clic en **Establecer permisos de visualización explícitos en la carpeta DFS** y luego selecciona **Configurar permisos de visualización**.

4.  Agrega o quita usuarios o grupos haciendo clic en **Agregar** o **Quitar**.

5.  Para permitir que los usuarios vean la carpeta DFS, selecciona el usuario o grupo y luego selecciona la casilla de verificación **Permitir**.

    Para ocultar la carpeta de un grupo o usuario, selecciona el usuario o grupo y luego selecciona la casilla de verificación **Denegar**.

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>Para controlar la visibilidad de carpeta mediante una línea de comandos

1.  Abre una ventana del símbolo del sistema en un servidor que tenga el servicio de rol **Sistema de archivos distribuido** o la característica **Herramientas del sistema de archivos distribuido**.

2.  Escribe el siguiente comando, donde *&lt;DFSPath&gt;* es la ruta de acceso de la carpeta DFS (vínculo), *<DOMAIN\\Account>* es el nombre de la cuenta de usuario o grupo y *(...)* se reemplaza con entradas de control de acceso (ACE) adicionales:

    ```
    dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
    ```

    Por ejemplo, para reemplazar los permisos existentes con permisos que permitan a los grupos Administradores del dominio y CONTOSO\\Trainers acceso de lectura (R) a la carpeta \\contoso.office\public\training, escribe el siguiente comando:

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace 
   ```

3. Para realizar tareas adicionales desde el símbolo del sistema, usa los siguientes comandos:


| Comando | Descripción |
|---|---|
|[Dfsutil property sd deny](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|Deniega a un grupo o usuario la capacidad de ver la carpeta.|
|[Dfsutil property sd reset](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |Quita todos los permisos de la carpeta.|
|[Dfsutil property sd revoke](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| Quita una ACE de grupo o usuario de la carpeta. |

## <a name="see-also"></a>Consulta también

-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Instalar DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [Usar permisos heredados con la enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)