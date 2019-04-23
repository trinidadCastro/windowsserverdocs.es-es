---
title: Configurar la detección automática de caché hospedada de cliente mediante el punto de conexión de servicio
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd77fc76a999517cb8372aec8dfad25b4dd5be3b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829726"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar la detección automática de caché hospedada de cliente mediante el punto de conexión de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con este procedimiento puede utilizar Directiva de grupo para habilitar y configurar el modo caché hospedada de BranchCache en el dominio\-unido los equipos que ejecutan la siguiente BranchCache\-compatibles con sistemas operativos de Windows.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Para configurar equipos unidos a un dominio que ejecutan Windows Server 2008 R2 o Windows 7, vea Windows Server 2008 R2 [BranchCache Deployment Guide](https://technet.microsoft.com/library/ee649232.aspx).

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Para usar la directiva de grupo para configurar a clientes para el modo caché hospedada

1. En un equipo en el que está instalado el rol de servidor Servicios de dominio de Active Directory, abra Administrador del servidor, seleccione el servidor Local, haga clic en **herramientas**y, a continuación, haga clic en **Group Policy Management**. Se abre la consola de administración de directivas de grupo.

2. En la consola de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Bosque:** *corp.contoso.com*, **dominios**, *corp.contoso.com*, **objetos de directiva de grupo**, donde *corp.contoso.com* es el nombre del dominio donde se encuentran las cuentas de equipo del cliente de BranchCache que desea configurar.

3. Derecha\-haga clic en **Group Policy Objects**y, a continuación, haga clic en **New**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo \(GPO\). Por ejemplo, si desea asignar al objeto el nombre Equipos cliente de BranchCache, escriba **Equipos cliente de BranchCache**. Haga clic en **Aceptar**.

4. En la consola de administración de directivas de grupo, asegúrese de que **Group Policy Objects** está seleccionado y en la derecha del panel de detalles\-haga clic en el GPO que acaba de crear. Por ejemplo, si el nombre de los equipos cliente de BranchCache de GPO secundario\-haga clic en **equipos cliente de BranchCache**. Haga clic en **Editar**. Se abre la consola del Editor de administración de directivas de grupo.

5. En la consola Editor de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Configuración del equipo**, **Directivas**, **Plantillas administrativas: Las definiciones de directiva \(archivos ADMX\) recuperado desde el equipo local**, **red**, **BranchCache**.

6. Haga clic en **BranchCache**y, a continuación, en el panel de detalles, haga doble\-haga clic en **activar BranchCache**. Se abre el cuadro de diálogo **Activar BranchCache**.
  
7.  En el cuadro de diálogo **Activar BranchCache**, haga clic en **Habilitado** y, a continuación, en **Aceptar**.

8. En la consola Editor de administración de directivas de grupo, asegúrese de que **BranchCache** es aún seleccionada y, a continuación, en el panel de detalles tipo double\-haga clic en **habilitar hospedado caché de la detección automática mediante la conexión de servicio Punto**. Se abre el cuadro de diálogo de configuración de directiva.

9. En el **habilitar hospedado caché de la detección automática mediante el punto de conexión de servicio** cuadro de diálogo, haga clic en **habilitado**y, a continuación, haga clic en **Aceptar**.

10. Para habilitar los equipos cliente para descargar y contenido de la caché del servidor de archivos de BranchCache\-en función de los servidores de contenido: En la consola Editor de administración de directivas de grupo, asegúrese de que **BranchCache** es aún seleccionada y, a continuación, en el panel de detalles tipo double\-haga clic en **BranchCache para archivos de red**. Se abre el cuadro de diálogo **Configurar BranchCache para archivos de red**. 
11. En el cuadro de diálogo **Configurar BranchCache para archivos de red**, haga clic en **Habilitado**. En **Opciones**, escriba un valor numérico, en milisegundos, para el tiempo máximo de latencia de red de ida y vuelta y, a continuación, haga clic en **Aceptar**.
  
    > [!NOTE]
    > De forma predeterminada, los equipos cliente almacenar en caché el contenido de los servidores de archivos si la latencia de ida y vuelta de red es mayor que 80 milisegundos.
  
12. Para configurar la cantidad de espacio en disco duro asignada en cada equipo cliente para la memoria caché de BranchCache: En la consola Editor de administración de directivas de grupo, asegúrese de que **BranchCache** es aún seleccionada y, a continuación, en el panel de detalles tipo double\-haga clic en **establecer el porcentaje de espacio en disco usado para la caché del equipo cliente**. Se abre el cuadro de diálogo **Establecer el porcentaje de espacio en disco usado por la memoria caché del equipo cliente**. Haga clic en **Habilitado** y, a continuación, en **Opciones** escriba un valor numérico que represente el porcentaje de espacio en disco duro utilizado en cada equipo cliente para la memoria caché de BranchCache. Haga clic en **Aceptar**.

13. Para especificar la antigüedad de forma predeterminada, en días, para que los segmentos son válidos en la caché de datos de BranchCache en los equipos cliente: En la consola Editor de administración de directivas de grupo, asegúrese de que **BranchCache** es aún seleccionada y, a continuación, en el panel de detalles tipo double\-haga clic en **establecer la duración para segmentos en la caché de datos**. El **establecer la duración para segmentos en la caché de datos** abre el cuadro de diálogo. Haga clic en **habilitado**y, a continuación, en el panel de detalles, escriba el número de días que prefiera. Haga clic en **Aceptar**.

14. Configurar opciones adicionales de directiva de BranchCache para los equipos cliente según corresponda para su implementación.

15. Actualizar directiva de grupo en los equipos cliente la sucursal, ejecute el comando **gpupdate /force**, o bien, reinicie el equipo de los equipos cliente.

La implementación en modo caché hospedada de BranchCache está completa ahora.

Para obtener más información sobre las tecnologías en esta guía, consulte [recursos adicionales](11-Bc-Hcm-additional-resources.md).