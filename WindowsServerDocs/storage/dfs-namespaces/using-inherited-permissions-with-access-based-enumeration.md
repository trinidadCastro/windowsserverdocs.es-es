---
title: Usar permisos heredados con la enumeración basada en el acceso
description: En este artículo se describe cómo usar permisos heredados con la enumeración basada en el acceso
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 433fe53a3d580aafc50b152ec20156436b05481f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402134"
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>Usar permisos heredados con la enumeración basada en el acceso

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

De manera predeterminada, los permisos que se usan para una carpeta DFS se heredan de sistema de archivos local del servidor de espacio de nombres. Los permisos se heredan del directorio raíz de la unidad del sistema y conceden permisos de lectura al dominio\\grupo usuarios. Como resultado, incluso después de habilitar la enumeración basada en el acceso, todas las carpetas del espacio de nombres permanecen visibles para todos los usuarios del dominio.

## <a name="advantages-and-limitations-of-inherited-permissions"></a>Ventajas y limitaciones de los permisos heredados

Hay dos ventajas principales para usar los permisos heredados para controlar qué usuarios pueden ver las carpetas de un espacio de nombres DFS:

-   Puedes aplicar rápidamente permisos heredados a muchas carpetas sin tener que usar scripts.
-   Puedes aplicar permisos heredados a raíces de espacio de nombres y a carpetas sin destinos.

A pesar de las ventajas, los permisos heredados de espacios de nombres DFS tienen muchas limitaciones que los hacen inapropiados para la mayoría de los entornos:

-   Las modificaciones en los permisos heredados no se replican a otros servidores de espacios de nombres. Por lo tanto, usa los permisos heredados solo en los espacios de nombres independientes o en los entornos en los que puedes implementar un sistema de replicación de terceros para mantener sincronizadas las listas de control de acceso (ACL) en todos los servidores de espacios de nombres.
-   Administración de DFS y **Dfsutil** no pueden ver ni modificar permisos heredados. Por lo tanto, debes usar el Explorador de Windows o el comando **Icacls**, además de la Administración de DFS o **Dfsutil** para administrar el espacio de nombres.
-   Al usar los permisos heredados, no puedes modificar los permisos de una carpeta con destinos excepto mediante el comando **Dfsutil**. Los espacios de nombres DFS quitan automáticamente los permisos de carpetas con destinos establecidos mediante otros métodos o herramientas.
-   Si estableces los permisos en una carpeta con destinos mientras usas permisos heredados, la ACL que estableces en la carpeta con destinos se combina con los permisos heredados del elemento principal de la carpeta del sistema de archivos. Debes examinar ambos conjuntos de permisos para determinar cuáles son los permisos de red.

> [!NOTE]
> Al usar los permisos heredados, es más sencillo establecer permisos en las raíces de espacio de nombres y las carpetas sin destinos. Luego usa los permisos heredados en carpetas con destinos para que hereden todos los permisos de sus elementos principales.

## <a name="using-inherited-permissions"></a>Usar permisos heredados

Para limitar qué usuarios pueden ver una carpeta DFS, debes realizar una de las siguientes tareas:

-   **Establezca permisos explícitos para la carpeta deshabilitando la herencia.** Para establecer permisos explícitos en una carpeta con destinos (un vínculo) mediante la opción de administración de DFS o el comando **Dfsutil**, consulta [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md).
-   **Modificar permisos heredados en el elemento principal del sistema de archivos local**. Para modificar los permisos heredados por una carpeta con destinos, si ya has establecido permisos explícitos en la carpeta, cambia a los permisos heredados de permisos explícitos, tal y como se explica en el siguiente procedimiento. Luego usa el Explorador de Windows o el comando **Icacls** para modificar los permisos de la carpeta desde la que la carpeta con destinos hereda sus permisos.

> [!NOTE]
> La enumeración basada en el acceso no impide que los usuarios obtengan una referencia a un destino de carpeta si ya conocen la ruta de acceso DFS de la carpeta con destinos. Los permisos establecidos mediante el Explorador de Windows o el comando **Icacls** en las raíces de espacio de nombres o las carpetas sin destinos controlan si los usuarios pueden acceder a la raíz de espacio de nombres o carpeta DFS. Sin embargo, no impiden que los usuarios accedan directamente a una carpeta con destinos. Solo los permisos de recursos compartidos o los permisos del sistema de archivos NTFS de la carpeta compartida pueden evitar que los usuarios tengan acceso a los destinos de carpeta.

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>Para cambiar de permisos explícitos a permisos heredados

1.  En el árbol de consola, en el nodo **Espacios de nombres**, busca la carpeta con destinos cuya visibilidad deseas controlar, haz clic en la carpeta y luego haz clic en **Propiedades**.

2.  Haga clic en la ficha **Opciones avanzadas**.

3.  Haz clic en **Usar permisos heredados del sistema de archivos local** y luego haz clic en **Aceptar** en el cuadro de diálogo **Confirmar uso de permisos heredados**. Esto también quita todos los permisos establecidos de forma explícita en esta carpeta, restaurando los permisos NTFS heredados del sistema de archivos local del servidor de espacio de nombres.

4.  Para cambiar los permisos heredados de carpetas o raíces de espacio de nombres de un espacio de nombres DFS, usa el Explorador de Windows o el comando **ICacls**.

## <a name="see-also"></a>Consulte también

-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md)