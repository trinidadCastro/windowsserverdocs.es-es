---
title: Configurar la detección automática de caché hospedada de cliente mediante el punto de conexión de servicio
description: Obtenga información acerca de cómo usar directiva de grupo para habilitar y configurar el modo caché hospedada de BranchCache en equipos Unidos a un dominio que ejecutan los siguientes sistemas operativos de Windows compatibles con BranchCache.
manager: brianlic
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 00962ad02e78a577e72b4ffd081696b86bbb11b7
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965617"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar la detección automática de caché hospedada de cliente mediante el punto de conexión de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con este procedimiento puede usar directiva de grupo para habilitar y configurar el modo caché hospedada de BranchCache en \- equipos Unidos a un dominio que ejecuten los siguientes \- sistemas operativos de Windows compatibles con BranchCache.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]
> Para configurar equipos Unidos a un dominio que ejecuten Windows Server 2008 R2 o Windows 7, vea la [Guía de implementación](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee649232(v=ws.10))de windows Server 2008 R2 BranchCache.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Para usar directiva de grupo para configurar clientes para el modo caché hospedada

1. En un equipo en el que esté instalado el rol de servidor Active Directory Domain Services, abra Administrador del servidor, seleccione el servidor local, haga clic en **herramientas** y, a continuación, haga clic en **Administración de directiva de grupo**. Se abre la Consola de administración de directivas de grupo.

2. En la consola de administración de directiva de grupo, expanda la siguiente ruta de acceso: **bosque:** *Corp.contoso.com*, **dominios**, *Corp.contoso.com* **Directiva de grupo objetos**, donde *Corp.contoso.com* es el nombre del dominio donde se encuentran las cuentas de equipo cliente de BranchCache que desea configurar.

3. Haga clic con el botón secundario \- en **Directiva de grupo objetos** y luego haga clic en **nuevo**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo GPO de objeto de directiva de grupo \( \) . Por ejemplo, si desea asignar al objeto el nombre Equipos cliente de BranchCache, escriba **Equipos cliente de BranchCache**. Haga clic en **Aceptar**.

4. En la consola de administración de directiva de grupo, asegúrese de que **Directiva de grupo objetos** esté seleccionado y, en el panel de detalles, haga clic con el botón secundario \- en el GPO que acaba de crear. Por ejemplo, si ha llamado a los equipos cliente de BranchCache de GPO, haga clic con el botón secundario \- en **equipos cliente de BranchCache**. Haga clic en **Editar**. Se abre la consola de Editor de administración de directivas de grupo.

5. En la consola de Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas** **plantillas administrativas: \( archivos ADMX \) de definiciones de directivas recuperados del equipo local**, **red**, **BranchCache**.

6. Haga clic en **BranchCache** y, a continuación, en el panel de detalles, \- haga doble clic en **Activar BranchCache**. Se abre el cuadro de diálogo **Activar BranchCache**.

7.  En el cuadro de diálogo **Activar BranchCache**, haga clic en **Habilitado** y, a continuación, en **Aceptar**.

8. En la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, \- haga doble clic en **Habilitar la detección automática de caché hospedada por punto de conexión de servicio**. Se abre el cuadro de diálogo Configuración de directiva.

9. En el cuadro de diálogo **Habilitar detección automática de caché hospedada por punto de conexión de servicio** , haga clic en **habilitado** y, a continuación, haga clic en **Aceptar**.

10. Para que los equipos cliente puedan descargar y almacenar en caché el contenido de los servidores de contenido basados en servidor de archivos de BranchCache \- : en la consola de editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, \- haga doble clic en **BranchCache para archivos de red**. Se abre el cuadro de diálogo **Configurar BranchCache para archivos de red**.
11. En el cuadro de diálogo **Configurar BranchCache para archivos de red**, haga clic en **Habilitado**. En **Opciones**, escriba un valor numérico, en milisegundos, para el tiempo máximo de latencia de red de ida y vuelta y, a continuación, haga clic en **Aceptar**.

    > [!NOTE]
    > De forma predeterminada, los equipos cliente almacenan en caché el contenido de los servidores de archivos si la latencia de red de ida y vuelta es mayor que 80 milisegundos.

12. Para configurar la cantidad de espacio en disco duro asignado en cada equipo cliente para la memoria caché de BranchCache: en la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, \- haga doble clic en **establecer el porcentaje de espacio en disco usado para la caché del equipo cliente**. Se abre el cuadro de diálogo **Establecer el porcentaje de espacio en disco usado por la memoria caché del equipo cliente**. Haga clic en **Habilitado** y, a continuación, en **Opciones** escriba un valor numérico que represente el porcentaje de espacio en disco duro utilizado en cada equipo cliente para la memoria caché de BranchCache. Haga clic en **Aceptar**.

13. Para especificar la edad predeterminada, en días, para la que los segmentos son válidos en la caché de datos de BranchCache en los equipos cliente: en la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, \- haga doble clic en **establecer Age para segmentos en la memoria caché de datos**. Se abre el cuadro de diálogo **establecer edad para segmentos en la caché de datos** . Haga clic en **habilitado** y, en el panel de detalles, escriba el número de días que prefiera. Haga clic en **Aceptar**.

14. Configure opciones de directiva de BranchCache adicionales para los equipos cliente según corresponda para su implementación.

15. Actualice directiva de grupo en equipos cliente de la sucursal ejecutando el comando **gpupdate/force** o reiniciando los equipos cliente.

La implementación del modo de caché hospedada de BranchCache ahora está completa.

Para obtener información adicional sobre las tecnologías de esta guía, consulte [recursos adicionales](11-Bc-Hcm-additional-resources.md).