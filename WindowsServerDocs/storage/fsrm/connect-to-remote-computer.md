---
title: Connect to a Remote Computer
description: En este artículo se describe cómo conectarse a un equipo remoto para administrar recursos de almacenamiento desde el servidor de archivos Administrador de recursos
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a412fcc0e1979ed158f01f6fba17fb6a9c4a20c7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935884"
---
# <a name="connect-to-a-remote-computer"></a>Connect to a Remote Computer

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para administrar recursos de almacenamiento en un equipo remoto, puede conectarse a un equipo desde el Administrador de recursos del servidor de archivos. Mientras está conectado, el servidor de archivos Administrador de recursos le permite administrar cuotas, archivos de pantalla, administrar clasificaciones, programar tareas de administración de archivos y generar informes con esos recursos remotos.

> [!Note]
> El Administrador de recursos del servidor de archivos puede administrar recursos en el equipo local o en un equipo remoto, pero no en los dos al mismo tiempo.

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>Para conectarse a un equipo remoto desde el Administrador de recursos del servidor de archivos

1.  En **herramientas administrativas**, haga clic en **servidor de archivos administrador de recursos**.

2.  En el árbol de consola, haga clic con el botón secundario en **Administrador de recursos del servidor de archivos** y, a continuación, haga clic en **Conectar a otro equipo**.

3.  En el cuadro de diálogo **Conectar a otro equipo**, haga clic en **Otro equipo**. A continuación, escriba el nombre del servidor al que desea conectarse (o haga clic en **examinar** para buscar un equipo remoto).

4.  Haga clic en **Aceptar**.

> [!Important]
> El comando **Conectar a otro equipo** sólo está disponible cuando se abre el Administrador de recursos del servidor de archivos desde **Herramientas administrativas**. Si se obtiene acceso al Administrador de recursos del servidor de archivos desde el Administrador de servidores, el comando no estará disponible.

## <a name="additional-considerations"></a>Consideraciones adicionales

Para administrar recursos remotos con el Administrador de recursos del servidor de archivos:

-   Debe haber iniciado sesión en el equipo local con una cuenta de dominio que sea miembro del grupo **administradores** en el equipo remoto.
-   El equipo remoto debe ejecutar Windows Server y el servidor de archivos Administrador de recursos debe estar instalado.
-   La excepción de **Administración de administrador de recursos del servidor de archivos remoto** en el equipo remoto debe estar habilitada. Habilite esta excepción mediante Firewall de Windows en el Panel de control.

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar recursos de almacenamiento remoto](managing-remote-storage-resources.md)