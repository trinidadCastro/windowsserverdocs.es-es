---
title: Conectarse a un equipo remoto
description: En este artículo se describe cómo conectarse a un equipo remoto para administrar recursos de almacenamiento desde el Administrador de recursos del servidor de archivos
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4d813933ec3073ecb3348468ca4b8f2e124c403d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402020"
---
# <a name="connect-to-a-remote-computer"></a>Conectarse a un equipo remoto 

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para administrar recursos de almacenamiento de un equipo remoto, puedes conectarte al equipo desde el Administrador de recursos del servidor de archivos. Mientras estás conectado, el Administrador de recursos del servidor de archivos te permite administrar cuotas, filtrar archivos, administrar clasificaciones, programar tareas de administración de archivos y generar informes con esos recursos remotos.

> [!Note]
> El Administrador de recursos del servidor de archivos puede administrar recursos tanto en el equipo local como en un equipo remoto, pero no en ambos al mismo tiempo.

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>Para conectarse a un equipo remoto desde el Administrador de recursos del servidor de archivos

1.  En **Herramientas administrativas**, haz clic en **Administrador de recursos del servidor de archivos**.

2.  En el árbol de consola, haz clic con el botón derecho en **Administrador de recursos del servidor de archivos** y luego haz clic en **Conectarse a otro equipo**.

3.  En el cuadro de diálogo **Conectarse a otro equipo**, haz clic en **Otro equipo**. Escribe el nombre del servidor al que quieres conectarte (o haz clic en **Examinar** para buscar un equipo remoto).

4.  Haga clic en **Aceptar**.

> [!Important]
> El comando **Conectarse a otro equipo** únicamente está disponible al abrir el Administrador de recursos del servidor de archivos desde **Herramientas administrativas**. Al acceder al Administrador de recursos del servidor de archivos desde el Administrador del servidor, el comando no está disponible.

## <a name="additional-considerations"></a>Consideraciones adicionales

Para administrar recursos remotos con el Administrador de recursos del servidor de archivos:

-   Debes iniciar sesión en el equipo local con una cuenta de dominio que forme parte del grupo de **Administradores** del equipo remoto.
-   El equipo remoto debe ejecutar Windows Server y debe tener instalado el Administrador de recursos del servidor de archivos.
-   La excepción **Administración del Administrador de recursos del servidor de archivos remoto** del equipo remoto debe estar habilitada. Habilita esta excepción usando el Firewall de Windows en el Panel de control.

## <a name="see-also"></a>Vea también

-   [Administrar recursos de almacenamiento remoto](managing-remote-storage-resources.md)