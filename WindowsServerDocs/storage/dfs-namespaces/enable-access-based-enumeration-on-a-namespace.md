---
title: Habilitar la enumeración basada en el acceso en un espacio de nombres
description: En este artículo se describe cómo habilitar la enumeración basada en el acceso en un espacio de nombres.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f011bc12c26567ed3a0e912dca3c3a8de9bfff9
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474932"
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>Habilitar la enumeración basada en el acceso en un espacio de nombres

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

La enumeración basada en el acceso oculta los archivos y carpetas a los que los usuarios no tienen permisos de acceso. De forma predeterminada, esta característica no está habilitada para los espacios de nombres DFS. Puede habilitar la enumeración basada en el acceso de las carpetas DFS mediante administración de DFS. Para controlar la enumeración basada en el acceso de archivos y carpetas en destinos de carpeta, debe habilitar la enumeración basada en el acceso en cada carpeta compartida mediante administración de almacenamiento y recursos compartidos.

Para habilitar la enumeración basada en el acceso en un espacio de nombres, todos los servidores de espacio de nombres deben ejecutar Windows Server 2008 o una versión más reciente. Además, los espacios de nombres basados en dominio deben usar el modo Windows Server 2008. Para obtener información acerca de los requisitos del modo Windows Server 2008, vea [elegir un tipo de espacio de nombres](choose-a-namespace-type.md).

En algunos entornos, la habilitación de la enumeración basada en el acceso puede causar un uso intensivo de la CPU en el servidor y tiempos de respuesta lentos para los usuarios.

> [!NOTE]
> Si actualiza el nivel funcional del dominio a Windows Server 2008 mientras haya espacios de nombres basados en el dominio existentes, la administración de DFS le permitirá habilitar la enumeración basada en el acceso en estos espacios de nombres. Sin embargo, no podrá editar permisos para ocultar carpetas de grupos o usuarios a menos que migre los espacios de nombres al modo Windows Server 2008. Para obtener más información, vea [migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).


Para usar la enumeración basada en el acceso con espacios de nombres DFS, debe seguir estos pasos:

-   Habilitar la enumeración basada en el acceso en un espacio de nombres
-   Controlar qué usuarios y grupos pueden ver carpetas DFS individuales


> [!WARNING]
> La enumeración basada en el acceso no impide que los usuarios obtengan una referencia a un destino de carpeta si ya conocen la ruta de acceso DFS. Solo los permisos del recurso compartido o del sistema de archivos NTFS del destino de la carpeta (carpeta compartida) pueden impedir que los usuarios tengan acceso a un destino de carpeta. Los permisos de la carpeta DFS solo se usan para mostrar u ocultar carpetas DFS, no para controlar el acceso, haciendo que el acceso de lectura sea el único permiso pertinente en el nivel de carpeta DFS. Para obtener más información, vea [usar permisos heredados con enumeración basada en el acceso](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx) .

<br />
Puede habilitar la enumeración basada en el acceso en un espacio de nombres mediante la interfaz de Windows o mediante una línea de comandos.

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>Para habilitar la enumeración basada en el acceso mediante la interfaz de Windows

1.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic con el botón secundario en el espacio de nombres adecuado y, a continuación, haga clic en **propiedades** .

2.  Haga clic en la pestaña **Opciones avanzadas** y, a continuación, active la casilla **Habilitar enumeración basada en el acceso para este espacio de nombres** .

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>Para habilitar la enumeración basada en el acceso mediante una línea de comandos

1.  Abra una ventana del símbolo del sistema en un servidor que tenga instalada la característica **sistema de archivos distribuido** servicio de rol o **sistema de archivos distribuido herramientas** .

2.  Escriba el siguiente comando, donde *<\_>raíz del espacio de nombres* es la raíz del espacio de nombres:

    ```
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> Para administrar la enumeración basada en el acceso en un espacio de nombres mediante Windows PowerShell, use los cmdlets [set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx), [Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx)y [REVOKE-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx) . El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.

Puede controlar qué usuarios y grupos pueden ver carpetas DFS individuales mediante la interfaz de Windows o mediante una línea de comandos.

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>Para controlar la visibilidad de carpetas mediante la interfaz de Windows

1.  En el árbol de consola, en el nodo **espacios de nombres** , busque la carpeta con destinos para los que desea controlar la visibilidad, haga clic con el botón secundario en ella y, a continuación, haga clic en **propiedades**.

2.  Haga clic en la ficha **Opciones avanzadas**.

3.  Haga clic en **establecer permisos de vista explícitos en la carpeta DFS** y, a continuación, **Configure ver permisos**.

4.  Agregue o quite grupos o usuarios haciendo clic en **Agregar** o **quitar**.

5.  Para permitir que los usuarios vean la carpeta DFS, seleccione el grupo o usuario y, a continuación, active la casilla **permitir** .

    Para ocultar la carpeta a un grupo o usuario, seleccione el grupo o usuario y, a continuación, active la casilla **denegar** .

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>Para controlar la visibilidad de carpetas mediante una línea de comandos

1. Abra una ventana del símbolo del sistema en un servidor que tenga instalada la característica **sistema de archivos distribuido** servicio de rol o **sistema de archivos distribuido herramientas** .

2. Escriba el siguiente comando, donde * &lt; DFSPath &gt; * es la ruta de acceso de la carpeta DFS (vínculo) *<\\ cuenta de dominio>* es el nombre de la cuenta de grupo o de usuario, y *(...)* se reemplaza por entradas de Access Control adicionales (ACE):

   ```
   dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
   ```

   Por ejemplo, para reemplazar los permisos existentes con permisos que permitan a los grupos Admins. del dominio y CONTOSO \\ Trainer Groups leer (R) el acceso a la \\ carpeta contoso. office\public\training, escriba el siguiente comando:

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace
   ```

3. Para realizar tareas adicionales desde el símbolo del sistema, use los comandos siguientes:


| Comando | Descripción |
|---|---|
|[Propiedad Dfsutil SD deny](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|Deniega a un grupo o usuario la capacidad de ver la carpeta.|
|[Propiedad Dfsutil de SD RESET](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |Quita todos los permisos de la carpeta.|
|[Propiedad Dfsutil SD REVOKE](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| Quita una ACE de grupo o de usuario de la carpeta. |

## <a name="additional-references"></a>Referencias adicionales

-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Instalación de DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [Usar permisos heredados con enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)