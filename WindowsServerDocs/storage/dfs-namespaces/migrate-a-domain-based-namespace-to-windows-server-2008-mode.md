---
title: Migrate a Domain-based Namespace to Windows Server 2008 Mode
description: En este artículo se describe cómo migrar un espacio de nombres basado en dominio al modo Windows Server 2008
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a7326584e2889fc0fd451b56ca4af065040f408d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942279"
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrar un espacio de nombres basado en dominio al modo Windows Server 2008

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

El modo de Windows Server 2008 para espacios de nombres basados en dominio incluye compatibilidad con la enumeración basada en el acceso y una mayor escalabilidad.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Para migrar un espacio de nombres basado en el dominio al modo Windows Server 2008

Para migrar un espacio de nombres basado en dominio del modo de servidor de Windows 2000 al modo 2008 de Windows Server, debe exportar el espacio de nombres a un archivo, eliminar el espacio de nombres, volver a crearlo en el modo de Windows Server 2008 y, a continuación, importar la configuración del espacio de nombres. Para ello, use el siguiente procedimiento:

1.  Abra una ventana del símbolo del sistema y escriba el siguiente comando para exportar el espacio de nombres a un archivo, donde el \\ \\ *domain* \\ *espacio de nombres* del dominio es el nombre del dominio adecuado, y el espacio de nombres y la *ruta de acceso \\ * son la ruta de acceso y el nombre del archivo para la exportación:
     ```
     Dfsutil root export \\domain\namespace path\filename.xml
     ```
2.  Anote la ruta de acceso ( \\ \\ *server* \\ *recurso compartido* de servidor) para cada servidor de espacio de nombres. Debe agregar manualmente los servidores de espacio de nombres al espacio de nombres que se ha vuelto a crear porque Dfsutil no puede importar servidores de espacio de nombres.
3.  En administración de DFS, haga clic con el botón secundario en el espacio de nombres y, a continuación, haga clic en **eliminar**o escriba el siguiente comando en el símbolo del sistema: <br /> donde \\ \\ *Domain* \\ *namespace* es el nombre del dominio y el espacio de nombres adecuados:
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  En administración de DFS, vuelva a crear el espacio de nombres con el mismo nombre, pero use el modo Windows Server 2008, o escriba el siguiente comando en un símbolo del sistema, donde <br /> \\\\*servidor* \\ de *espacio de nombres* es el nombre del servidor y el recurso compartido adecuados para la raíz del espacio de nombres:
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Para importar el espacio de nombres del archivo de exportación, escriba el siguiente comando en un símbolo del sistema, donde <br /> \\\\*dominio* \\ de *espacio de nombres* es el nombre del dominio y el espacio de nombres adecuados, y la ruta de acceso es el nombre de archivo y la ruta de acceso del archivo que se va a importar: * \\ *
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Para minimizar el tiempo necesario para importar un espacio de nombres grande, ejecute el comando **Dfsutil** root Import localmente en un servidor de espacio de nombres.
6.  Agregue los demás servidores de espacio de nombres al espacio de nombres que se ha vuelto a crear; para ello, haga clic con el botón secundario en el espacio de nombres en administración de DFS y, a continuación, haga clic en **Agregar servidor de espacio de nombres**, o bien escriba el siguiente comando <br /> \\\\*servidor* \\ de *recurso compartido* es el nombre del servidor y el recurso compartido adecuados para la raíz del espacio de nombres:
     ```
     Dfsutil target add \\server\share
     ```

    > [!NOTE]
    > Puede agregar servidores de espacio de nombres antes de importar el espacio de nombres, pero hacerlo hace que los servidores de espacio de nombres descarguen incrementalmente los metadatos para el espacio de nombres en lugar de descargar inmediatamente todo el espacio de nombres después de agregarlo como un servidor de espacio de nombres.

## <a name="additional-references"></a>Referencias adicionales
-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md)