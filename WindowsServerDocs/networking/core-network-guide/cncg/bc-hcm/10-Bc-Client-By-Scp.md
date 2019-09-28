---
title: Configurar la detección automática de caché hospedada de cliente mediante el punto de conexión de servicio
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ccc8b80537da0d0b689f6c508c75ef15a339c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406394"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar la detección automática de caché hospedada de cliente mediante el punto de conexión de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con este procedimiento puede usar directiva de grupo para habilitar y configurar el modo de caché hospedada de BranchCache en los equipos de dominio @ no__t-0joined que ejecutan los siguientes sistemas operativos Windows de BranchCache @ no__t-1capable.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Para configurar equipos Unidos a un dominio que ejecuten Windows Server 2008 R2 o Windows 7, vea la [Guía de implementación](https://technet.microsoft.com/library/ee649232.aspx)de windows Server 2008 R2 BranchCache.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Para usar directiva de grupo para configurar clientes para el modo caché hospedada

1. En un equipo en el que esté instalado el rol de servidor Active Directory Domain Services, abra Administrador del servidor, seleccione el servidor local, haga clic en **herramientas**y, a continuación, haga clic en **Administración de directiva de grupo**. Se abre la consola de administración de directiva de grupo.

2. En la consola de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Bosque:** *Corp.contoso.com*, **dominios**, *Corp.contoso.com*, **Directiva de grupo objetos**, donde *Corp.contoso.com* es el nombre del dominio en el que las cuentas de equipo cliente de BranchCache desea Configure.

3. Right @ no__t-0click **Directiva de grupo objetos**y, a continuación, haga clic en **nuevo**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo \(GPO @ no__t-2. Por ejemplo, si desea asignar al objeto el nombre Equipos cliente de BranchCache, escriba **Equipos cliente de BranchCache**. Haga clic en **Aceptar**.

4. En la consola de administración de directiva de grupo, asegúrese de que **Directiva de grupo objetos** esté seleccionado y, en el panel de detalles, haga clic en @ no__t-1CLICK el GPO que acaba de crear. Por ejemplo, si ha llamado a los equipos cliente de BranchCache del GPO, haga clic en **los equipos cliente de BranchCache**@ no__t-0click. Haga clic en **Editar**. Se abre la consola de Editor de administración de directivas de grupo.

5. En la consola de Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **Configuración del equipo**, **Directivas**, **Plantillas administrativas: Definiciones de directiva \(ADMX archivos @ no__t-1 recuperados del equipo local @ no__t-2, **red**, **BranchCache**.

6. Haga clic en **BranchCache**y, a continuación, en el panel de detalles, haga doble @ no__t-1Click **Active BranchCache**. Se abre el cuadro de diálogo **Activar BranchCache**.
  
7.  En el cuadro de diálogo **Activar BranchCache**, haga clic en **Habilitado** y, a continuación, en **Aceptar**.

8. En la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** sigue seleccionado y, a continuación, en el panel de detalles, haga doble @ no__t-1Click **Habilitar detección automática de caché hospedada por punto de conexión de servicio**. Se abre el cuadro de diálogo Configuración de directiva.

9. En el cuadro de diálogo **Habilitar detección automática de caché hospedada por punto de conexión de servicio** , haga clic en **habilitado**y, a continuación, haga clic en **Aceptar**.

10. Para habilitar los equipos cliente para descargar y almacenar en caché el contenido del servidor de archivos de BranchCache @ no__t-0based servidores de contenido: En la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, haga doble @ no__t-1Click **BranchCache para archivos de red**. Se abre el cuadro de diálogo **Configurar BranchCache para archivos de red**. 
11. En el cuadro de diálogo **Configurar BranchCache para archivos de red**, haga clic en **Habilitado**. En **Opciones**, escriba un valor numérico, en milisegundos, para el tiempo máximo de latencia de red de ida y vuelta y, a continuación, haga clic en **Aceptar**.
  
    > [!NOTE]
    > De forma predeterminada, los equipos cliente almacenan en caché el contenido de los servidores de archivos si la latencia de red de ida y vuelta es mayor que 80 milisegundos.
  
12. Para configurar la cantidad de espacio en disco duro asignada en cada equipo cliente para la memoria caché de BranchCache: En la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, haga doble @ no__t-1Click **establezca el porcentaje de espacio en disco usado para la caché del equipo cliente**. Se abre el cuadro de diálogo **Establecer el porcentaje de espacio en disco usado por la memoria caché del equipo cliente**. Haga clic en **Habilitado** y, a continuación, en **Opciones** escriba un valor numérico que represente el porcentaje de espacio en disco duro utilizado en cada equipo cliente para la memoria caché de BranchCache. Haga clic en **Aceptar**.

13. Para especificar la edad predeterminada, en días, para la que los segmentos son válidos en la caché de datos de BranchCache en los equipos cliente: En la consola de Editor de administración de directivas de grupo, asegúrese de que **BranchCache** todavía está seleccionado y, a continuación, en el panel de detalles, haga doble @ no__t-1Click **establezca Age para segmentos en la memoria caché de datos**. Se abre el cuadro de diálogo **establecer edad para segmentos en la caché de datos** . Haga clic en **habilitado**y, en el panel de detalles, escriba el número de días que prefiera. Haga clic en **Aceptar**.

14. Configure opciones de directiva de BranchCache adicionales para los equipos cliente según corresponda para su implementación.

15. Actualice directiva de grupo en equipos cliente de la sucursal ejecutando el comando **gpupdate/force**o reiniciando los equipos cliente.

La implementación del modo de caché hospedada de BranchCache ahora está completa.

Para obtener información adicional sobre las tecnologías de esta guía, consulte [recursos adicionales](11-Bc-Hcm-additional-resources.md).