---
title: Administrar aplicaciones en Windows Server Essentials
description: Obtenga información acerca de cómo administrar aplicaciones e instalar y quitar complementos en Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: f978405236a83c89f07e65589e5d9af899ea1045
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755301"
---
# <a name="manage-applications-in-windows-server-essentials"></a>Administrar aplicaciones en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 El panel del servidor en Windows Server Essentials y Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado permite realizar tareas administrativas comunes. Para realizar estas tareas, vea lo siguiente:

-   [Tareas de administración de la aplicación en el panel](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)

-   [Instalar o quitar complementos mediante el panel](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)

##  <a name="application-management-tasks-in-the-dashboard"></a><a name="BKMK_1"></a> Tareas de administración de aplicaciones en el panel
 En la página de administración **Aplicaciones** del panel encontrará lo siguiente:

- Una lista de complementos instalados, que muestra:

  -   El nombre del servicio en línea o complemento

  -   El estado de actualización del complemento

  -   El estado de suscripción del complemento

  -   El nombre de la empresa o el editor que ofrece el complemento

- Un panel que incluye un conjunto de tareas para administrar un complemento determinado

- Una lista de complementos disponibles para descargar e instalar desde Microsoft Pinpoint

  En la tabla siguiente se describen las diversas tareas de administración del complemento que están disponibles en el panel del servidor. Algunas de las tareas son para complementos, por lo que solo están visibles cuando se selecciona un complemento en la lista.

|Nombre de la tarea|Description|
|---------------|-----------------|
|Quitar el complemento|Quita el complemento seleccionado desde el servidor y de todos los demás equipos de la red.|
|Instalar el complemento en los equipos de la red|Ayuda a programar la instalación del complemento seleccionado en todos los equipos de la red.|
|Obtener ayuda sobre el complemento|Se abre un sitio web en el explorador desde el que puede buscar soluciones a problemas y obtener más información sobre un complemento.|
|Actualizar el complemento|Ayuda a descargar e instalar actualizaciones para los complementos que están instalados en el servidor y equipos de la red.|
|Renovar la suscripción al complemento|Se abre un sitio web en el explorador desde el que puede renovar la suscripción al complemento.|
|Leer la declaración de privacidad para el complemento|Se abre un sitio web en el explorador en el que podrá leer la declaración de privacidad.|
|¿Cómo instalo o quito complementos?|Abre una página web en el explorador que muestra el tema de ayuda correspondiente.|

##  <a name="install-or-remove-add-ins-using-the-dashboard"></a><a name="BKMK_2"></a> Instalar o quitar complementos mediante el panel
 Un complemento es una aplicación de software que ofrece características adicionales y funciones para servidores. Hay cada vez más complementos de Microsoft y de otros proveedores de software independientes (ISV).

 Para aprovechar las funciones extendidas de un complemento, debe instalar el complemento en el servidor.

#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Para instalar un complemento de Microsoft Pinpoint

1.  En el panel del servidor, haga clic en **aplicaciones** y, a continuación, haga clic en la pestaña **Microsoft Pinpoint** .  Aparece una lista de complementos disponibles.

2.  Haga clic en el complemento que quiera instalar. Aparecerá la página de información.

3.  En la página de información del complemento, haga clic en Descargar y siga las instrucciones en pantalla para descargar e instalar el complemento.

4.  Siga las instrucciones del asistente para instalar el complemento.

5.  Cuando finalice la instalación, reinicie el panel, abra la página **Aplicaciones** del panel del servidor y compruebe que el complemento aparezca en la vista de lista.

#### <a name="to-install-an-add-in-from-another-provider"></a>Para instalar un complemento de otro proveedor

1.  Abra el Explorador de Windows y busque la ubicación del archivo de instalación del complemento.

2.  Haga doble clic en el archivo para ejecutar al asistente de instalación.

3.  Siga las instrucciones del asistente para instalar el complemento.

4.  Cuando finalice la instalación, reinicie el panel, abra la página **Aplicaciones** y, a continuación, compruebe que el complemento aparezca en la vista de lista.

#### <a name="to-remove-an-add-in"></a>Para quitar un complemento

1.  Abra el panel del servidor.

2.  Haga clic en la pestaña **Aplicaciones**.

3.  En la pestaña **Complementos**, seleccione el complemento que quiera quitar y, a continuación, haga clic en **Quitar el complemento**.

4.  En la ventana **Quitar complemento**, haga clic en **Quitar**.

    > [!NOTE]
    >  Deberá reiniciar el panel para quitar el complemento de forma permanente.

## <a name="additional-references"></a>Referencias adicionales

-   [Información general del panel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
