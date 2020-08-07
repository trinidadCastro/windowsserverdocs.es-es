---
title: Configurar el objeto de directiva de grupo Publicación de hash para BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c139a85e73bcb1f64e752cb2763032003486843a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971912"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurar el objeto de directiva de grupo Publicación de hash para BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar el objeto de directiva de grupo de publicación de hash de BranchCache (GPO) para que todos los servidores de archivos que ha agregado a la unidad organizativa tengan aplicada la misma configuración de directiva de publicación de hash.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

> [!NOTE]
> Antes de llevar a cabo este procedimiento, debe crear la unidad organizativa Servidores de archivos de BranchCache, migrar los servidores de archivos a la unidad organizativa y crear el GPO publicación de hash de BranchCache. Para obtener más información, vea [Habilitar la publicación de hash para servidores de archivos de miembros de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).

### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Para configurar el objeto de directiva de grupo de publicación de hash de BranchCache

1.  Ejecute Windows PowerShell como administrador, escriba **MMC**y, a continuación, presione Entrar. Se abrirá Microsoft Management Console (MMC).

2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.

3.  En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Administración de directivas de grupo** y, a continuación, haga clic en **Aceptar**.

4.  En MMC de Administración de directivas de grupo, expanda la ruta de acceso al objeto de directiva de grupo Publicación de hash para BranchCache que creó previamente. Por ejemplo, si el bosque se denomina example.com, el dominio se denomina example1.com y el GPO se llama **publicación de hash de BranchCache**, expanda la ruta de acceso siguiente: **Directiva de grupo Management**, **Forest: example.com**, **Domains**, **example1.com**, **Directiva de grupo Objects**, **BranchCache hash Publication**.

5.  Haga clic con el botón secundario en el objeto de directiva de grupo **Publicación de hash para BranchCache** y haga clic en **Editar**. Se abre la consola de Editor de administración de directivas de grupo.

6.  En la consola de Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **plantillas administrativas**, **red**, **servidor LanMan**.

7.  En la consola de Editor de administración de directivas de grupo, haga clic en **Servidor Lanman**. En el panel de detalles, haga doble clic en **Publicación de hash para BranchCache**. Se abre el cuadro de diálogo **Publicación de hash para BranchCache**.

8.  En el cuadro de diálogo **Publicación de hash para BranchCache**, haga clic en **Habilitado**.

9. En **Opciones**, haga clic en **permitir la publicación de hash para todas las carpetas compartidas**y, a continuación, haga clic en una de las opciones siguientes:

    1.  Para habilitar la publicación de hash para todas las carpetas compartidas de todos los servidores de archivos que ha agregado a la unidad organizativa, haga clic en **permitir publicación de hash para todas las carpetas compartidas**.

    2.  Para habilitar la publicación de hash solamente para las carpetas compartidas para las que se ha habilitado BranchCache, haga clic en **Permitir la publicación de hash solo para carpetas compartidas en las que BranchCache está habilitado**.

    3.  Para no permitir la publicación de hash para todas las carpetas compartidas del equipo aunque se haya habilitado BranchCache en los recursos compartidos de archivos, haga clic en **No permitir la publicación de hash en ninguna carpeta compartida**.

10. Haga clic en **Aceptar**.

> [!NOTE]
> En la mayoría de los casos, debe guardar la consola MMC y actualizar la vista para que se muestren los cambios de configuración realizados.



