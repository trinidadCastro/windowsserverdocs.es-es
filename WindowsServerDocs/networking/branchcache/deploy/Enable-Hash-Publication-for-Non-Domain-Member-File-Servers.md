---
title: Habilitar la publicación de hash para servidores de archivos que no son miembros del dominio
description: Obtenga información sobre cómo configurar la publicación de hash para BranchCache mediante el directiva de grupo del equipo local en un servidor de archivos con el servicio de rol BranchCache para archivos de red del rol de servidor servicios de archivo instalado.
manager: brianlic
ms.topic: how-to
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 9da8c5677f3dd137fb682a17adc55da29f973f82
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948441"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Habilitar la publicación de hash para servidores de archivos que no son miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar la publicación de hash para BranchCache mediante el directiva de grupo del equipo local en un servidor de archivos que ejecute Windows Server 2016 con el servicio de rol **BranchCache para archivos de red** del rol de servidor servicios de archivo instalado.

Este procedimiento está previsto para su uso en un servidor de archivos que no sea miembro del dominio. Si realiza este procedimiento en un servidor de archivos que es miembro del dominio y también configura BranchCache utilizando la directiva de grupo de dominio, la configuración de esta directiva invalida la configuración de la directiva de grupo local.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.

> [!NOTE]
> Si tiene uno o más servidores de archivos que son miembros del dominio, puede agregarlos a una unidad organizativa (OU) en Servicios de dominio de Active Directory y utilizar después la directiva de grupo para configurar de una vez la publicación de hash para todos los servidores de archivos, en lugar de configurar individualmente cada servidor de archivos. Para obtener más información, vea [Habilitar la publicación de hash para servidores de archivos de miembros de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).

### <a name="to-enable-hash-publication-for-one-file-server"></a>Para habilitar la publicación de hash para un servidor de archivos

1.  Abra Windows PowerShell, escriba **mmv** y presione ENTRAR. Se abrirá Microsoft Management Console (MMC).

2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.

3.  En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Editor de objetos de directiva de grupo**. Se abre el Asistente para directivas de grupo con el objeto Equipo local seleccionado. Haz clic en **Finalizar** y, a continuación, en **Aceptar**.

4.  En el Editor de directivas de grupo local MMC, expanda la siguiente ruta de acceso: **Directiva de equipo local**, **configuración del equipo**, **plantillas administrativas**, **red**, **servidor LanMan**. Haga clic en **Servidor Lanman**.

5.  En el panel de detalles, haga doble clic en **Publicación de hash para BranchCache**. Se abre el cuadro de diálogo **Publicación de hash para BranchCache**.

6.  En el cuadro de diálogo **Publicación de hash para BranchCache**, haga clic en **Habilitado**.

7.  En **Opciones**, haga clic en **permitir la publicación de hash para todas las carpetas compartidas** y, a continuación, haga clic en una de las opciones siguientes:

    1.  Para habilitar la publicación de hash para todas las carpetas compartidas de este equipo, haga clic en **permitir publicación de hash para todas las carpetas compartidas**.

    2.  Para habilitar la publicación de hash solamente para las carpetas compartidas para las que se ha habilitado BranchCache, haga clic en **Permitir la publicación de hash solo para carpetas compartidas en las que BranchCache está habilitado**.

    3.  Para no permitir la publicación de hash para todas las carpetas compartidas del equipo aunque se haya habilitado BranchCache en los recursos compartidos de archivos, haga clic en **No permitir la publicación de hash en ninguna carpeta compartida**.

8.  Haga clic en **Aceptar**.



