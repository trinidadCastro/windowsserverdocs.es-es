---
title: Migrar un espacio de nombres basado en dominio al modo Windows Server 2008
description: En este artículo se describe cómo migrar un espacio de nombres basado en dominio al modo Windows Server 2008
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2624e53d306dd198525b0abe8adf96fe6060bc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847866"
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrar un espacio de nombres basado en dominio al modo Windows Server 2008

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

El modo Windows Server 2008 para espacios de nombres basados en dominio incluye compatibilidad con la enumeración basada en el acceso y mayor escalabilidad.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Para migrar un espacio de nombres basado en dominio al modo Windows Server 2008

Para migrar un espacio de nombres basado en dominio desde el modo Windows 2000 Server al modo Windows Server 2008, debes exportar el espacio de nombres a un archivo, eliminar el espacio de nombres, volver a crearlo en el modo Windows Server 2008 y luego importar su configuración. Para ello, utiliza el procedimiento siguiente:

1.  Abra una ventana del símbolo del sistema y escriba el siguiente comando para exportar el espacio de nombres en un archivo, donde \\ \\ *dominio*\\*espacio de nombres* es el nombre de la dominio adecuado y el espacio de nombres y *ruta\\filename* es la ruta de acceso y el nombre del archivo de exportación:
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Anote la ruta de acceso ( \\ \\ *server* \\ *compartir* ) para cada servidor de espacio de nombres. Debes agregar manualmente los servidores de espacio de nombres para el espacio de nombres que has vuelto a crear porque Dfsutil no puede importar servidores de espacio de nombres.
3.  En la opción de administración DFS, haz clic con el botón derecho en el espacio de nombres y luego haz clic en **Eliminar** o escribe el siguiente comando en un símbolo del sistema, <br /> donde \\ \\ *dominio*\\*espacio de nombres* es el nombre de dominio y el correspondiente espacio de nombres:
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  En la opción de administración DFS, vuelve a crear el espacio de nombres con el mismo nombre, pero usa el modo Windows Server 2008 o escribe el siguiente comando en un símbolo del sistema, donde <br /> \\\\*servidor*\\*espacio de nombres* es el nombre del servidor y recurso compartido para la raíz del espacio de nombres:
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Para importar el espacio de nombres de archivo de exportación, escribe el siguiente comando en un símbolo del sistema, donde <br /> \\\\*dominio*\\*espacio de nombres* es el nombre de dominio y el correspondiente espacio de nombres y *ruta\\filename* es la ruta de acceso y el nombre del archivo para importar:
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Para minimizar el tiempo necesario para importar un espacio de nombres de gran tamaño, ejecuta el comando de importación de raíz **Dfsutil** a nivel local en un servidor de espacio de nombres.
6.  Agrega los servidores de espacio de nombres restantes al espacio de nombres que has vuelto a crear haciendo clic con el botón derecho en el espacio de nombres en la opción de administración DFS y haciendo clic en **Agregar servidor de espacio de nombres** o escribiendo el siguiente comando en un símbolo del sistema, donde <br /> \\\\*servidor*\\*compartir* es el nombre del servidor y recurso compartido para la raíz del espacio de nombres:
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Puedes agregar servidores de espacio de nombres antes de importar el espacio de nombres, pero si lo haces los servidores de espacio de nombres descargarán de forma incremental los metadatos del espacio de nombres en lugar de descargar inmediatamente todo el espacio de nombres después de agregarse como servidor de espacio de nombres.

## <a name="see-also"></a>Vea también
-   [Implementación de espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Elija un tipo de Namespace](choose-a-namespace-type.md)