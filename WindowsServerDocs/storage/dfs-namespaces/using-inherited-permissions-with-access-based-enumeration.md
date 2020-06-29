---
title: Usar permisos heredados con la enumeración basada en el acceso
description: En este artículo se describe cómo usar permisos heredados con enumeración basada en el acceso
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 112ec4363177ac6dd560493843c8937bdfbac4de
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475152"
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>Usar permisos heredados con enumeración basada en el acceso

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

De forma predeterminada, los permisos usados para una carpeta DFS se heredan del sistema de archivos local del servidor de espacio de nombres. Los permisos se heredan del directorio raíz de la unidad del sistema y conceden \\ permisos de lectura al grupo usuarios del dominio. Como resultado, incluso después de habilitar la enumeración basada en el acceso, todas las carpetas del espacio de nombres permanecen visibles para todos los usuarios del dominio.

## <a name="advantages-and-limitations-of-inherited-permissions"></a>Ventajas y limitaciones de los permisos heredados

Hay dos ventajas principales en el uso de permisos heredados para controlar qué usuarios pueden ver las carpetas en un espacio de nombres DFS:

-   Puede aplicar rápidamente los permisos heredados a muchas carpetas sin tener que usar scripts.
-   Puede aplicar permisos heredados a raíces de espacio de nombres y carpetas sin destinos.

A pesar de las ventajas, los permisos heredados en espacios de nombres DFS tienen muchas limitaciones que los hacen inadecuados para la mayoría de los entornos:

-   Las modificaciones en los permisos heredados no se replican en otros servidores de espacio de nombres. Por lo tanto, use permisos heredados solo en espacios de nombres independientes o en entornos en los que pueda implementar un sistema de replicación de terceros para mantener sincronizadas las listas de Access Control (ACL) en todos los servidores de espacio de nombres.
-   Administración de DFS y **Dfsutil** no pueden ver ni modificar los permisos heredados. Por lo tanto, debe usar el explorador de Windows o el comando **icacls** además de la administración de DFS o **Dfsutil** para administrar el espacio de nombres.
-   Cuando se usan permisos heredados, no se pueden modificar los permisos de una carpeta con destinos, excepto mediante el comando **Dfsutil** . Los espacios de nombres DFS quitan automáticamente los permisos de las carpetas con destinos establecidos mediante otras herramientas o métodos.
-   Si establece permisos en una carpeta con destinos mientras usa permisos heredados, la ACL que establezca en la carpeta con destinos se combinará con los permisos heredados de la carpeta primaria del sistema de archivos. Debe examinar ambos conjuntos de permisos para determinar cuáles son los permisos de red.

> [!NOTE]
> Cuando se usan permisos heredados, es más sencillo establecer permisos en las raíces y carpetas de los espacios de nombres sin destinos. A continuación, use permisos heredados en carpetas con destinos para que hereden todos los permisos de sus elementos primarios.

## <a name="using-inherited-permissions"></a>Usar permisos heredados

Para limitar los usuarios que pueden ver una carpeta DFS, debe realizar una de las siguientes tareas:

-   **Establezca permisos explícitos para la carpeta deshabilitando la herencia.** Para establecer permisos explícitos en una carpeta con destinos (un vínculo) mediante administración de DFS o el comando **Dfsutil** , vea [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md).
-   **Modifique los permisos heredados en el elemento primario del sistema de archivos local**. Para modificar los permisos heredados por una carpeta con destinos, si ya ha establecido permisos explícitos en la carpeta, cambie a los permisos heredados de permisos explícitos, como se describe en el procedimiento siguiente. A continuación, use el explorador de Windows o el comando **icacls** para modificar los permisos de la carpeta de la que la carpeta con destinos hereda sus permisos.

> [!NOTE]
> La enumeración basada en el acceso no impide que los usuarios obtengan una referencia a un destino de carpeta si ya conocen la ruta de acceso DFS de la carpeta con destinos. Los permisos que se establecen con el explorador de Windows o el comando **icacls** en raíces de espacio de nombres o carpetas sin destinos controlan si los usuarios pueden tener acceso a la carpeta DFS o a la raíz del espacio de nombres. Sin embargo, no impiden que los usuarios accedan directamente a una carpeta con destinos. Solo los permisos de recurso compartido o los permisos del sistema de archivos NTFS de la carpeta compartida pueden impedir que los usuarios tengan acceso a los destinos de carpeta.

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>Para cambiar de permisos explícitos a permisos heredados

1.  En el árbol de la consola, en el nodo **espacios de nombres** , busque la carpeta con destinos cuya visibilidad desee controlar, haga clic con el botón secundario en la carpeta y, a continuación, haga clic en **propiedades**.

2.  Haga clic en la ficha **Opciones avanzadas**.

3.  Haga clic en **usar permisos heredados del sistema de archivos local** y, a continuación, haga clic en **Aceptar** en el cuadro de diálogo **confirmar uso de permisos heredados** . Al hacerlo, se quitan todos los permisos establecidos explícitamente en esta carpeta y se restauran los permisos NTFS heredados del sistema de archivos local del servidor de espacio de nombres.

4.  Para cambiar los permisos heredados de carpetas o raíces de espacio de nombres en un espacio de nombres DFS, use el explorador de Windows o el comando **ICacls** .

## <a name="additional-references"></a>Referencias adicionales

-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md)