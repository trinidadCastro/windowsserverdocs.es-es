---
title: Usar la directiva de grupo para configurar equipos cliente miembros del dominio
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.date: 06/02/2018
ms.openlocfilehash: 8e82d3e0ee7a84fbd6e2916d0f22472a8c117688
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855086"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Usar la directiva de grupo para configurar equipos cliente miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta sección, se crea un objeto de directiva de grupo para todos los equipos de su organización, configuración equipos cliente miembros del dominio con el modo caché distribuida o modo caché hospedada y configura Firewall de Windows con seguridad avanzada para permitir que BranchCache tráfico.  
  
Esta sección contiene los procedimientos siguientes.  
  
1.  [Para crear un objeto de directiva de grupo y configurar los modos de BranchCache](#bkmk_gp)  
  
2.  [Para configurar Firewall de Windows con reglas de tráfico de entrada de seguridad de avanzada](#bkmk_inbound)  
  
3.  [Para configurar Firewall de Windows con reglas de tráfico saliente de seguridad avanzada](#bkmk_outbound)  
  
> [!TIP]  
> En el siguiente procedimiento, cree un objeto de directiva de grupo en la directiva predeterminada de dominio que se indique, sin embargo, puede crear el objeto en una unidad organizativa (OU) o en otro contenedor que sea adecuado para su implementación.  
  
Debe ser miembro del **Admins. del dominio**, o equivalente para realizar estos procedimientos.  
  
## <a name="bkmk_gp"></a>Para crear un objeto de directiva de grupo y configurar los modos de BranchCache  
  
1.  En un equipo en el que está instalado el rol de servidor Servicios de dominio de Active Directory, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **Group Policy Management**. Se abre la consola de administración de directivas de grupo.  
  
2.  En la consola de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Bosque:** *ejemplo.com*, **dominios**, *ejemplo.com*, **objetos de directiva de grupo**, donde  *ejemplo.com* es el nombre del dominio donde se encuentran las cuentas de equipo del cliente de BranchCache que desea configurar.  
  
3.  Haga clic con el botón secundario en **Objetos de directiva de grupo** y, a continuación, haga clic en **Nuevo**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo (GPO). Por ejemplo, si desea asignar al objeto el nombre Equipos cliente de BranchCache, escriba **Equipos cliente de BranchCache**. Haga clic en **Aceptar**.  
  
4.  En la Consola de administración de directivas de grupo, asegúrese de que esté seleccionada la opción **Objetos de directiva de grupo** y, en el panel de detalles, haga clic con el botón secundario en el objeto de directiva de grupo que acaba de crear. Por ejemplo, si ha asignado al objeto de directiva de grupo el nombre Equipos cliente de BranchCache, haga clic con el botón secundario en **Equipos cliente de BranchCache**. Haga clic en **Editar**. Se abre la consola del Editor de administración de directivas de grupo.  
  
5.  En la consola Editor de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Configuración del equipo**, **Directivas**, **Plantillas administrativas: Recuperan las definiciones de directiva (archivos ADMX) desde el equipo local**, **red**, **BranchCache**.  
  
6.  Haga clic en **BranchCache** y, a continuación, en el panel de detalles, haga doble clic en **Activar BranchCache**. Se abre el cuadro de diálogo de configuración de directiva.  
  
7.  En el cuadro de diálogo **Activar BranchCache**, haga clic en **Habilitado** y, a continuación, en **Aceptar**.  
  
8.  Para habilitar el modo de caché distribuida de BranchCache, en el panel de detalles, haga doble clic en **modo caché distribuida BranchCache establecer**. Se abre el cuadro de diálogo de configuración de directiva.  
  
9. En el cuadro de diálogo **Establecer el modo Caché distribuida de BranchCache**, haga clic en **Habilitado** y, a continuación, haga clic en **Aceptar**.  
  
10. Si tiene uno o más sucursales, donde va a implementar BranchCache en modo caché hospedada y se hayan implementado los servidores de caché en las oficinas hospedada, haga doble clic en **habilitar hospedado caché de la detección automática mediante el punto de conexión de servicio**. Se abre el cuadro de diálogo de configuración de directiva.  
  
11. En el **habilitar hospedado caché de la detección automática mediante el punto de conexión de servicio** cuadro de diálogo, haga clic en **habilitado**y, a continuación, haga clic en **Aceptar**.  
  
    > [!NOTE]  
    > Cuando habilita tanto la **modo establecer caché distribuida de BranchCache** y **habilitar hospedado caché de la detección automática mediante el punto de conexión de servicio** operan de configuración de directiva, los equipos cliente de BranchCache caché distribuida, a menos que encuentre un servidor de caché hospedada en la sucursal, momento en que operan en modo caché hospedada.  
  
12. Utilice los procedimientos siguientes para configurar el firewall en los equipos cliente mediante la directiva de grupo.  
  
## <a name="bkmk_inbound"></a>Para configurar Firewall de Windows con reglas de tráfico de entrada de seguridad de avanzada  
  
1.  En la consola de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Bosque:** *ejemplo.com*, **dominios**, *ejemplo.com*, **objetos de directiva de grupo**, donde  *ejemplo.com* es el nombre del dominio donde se encuentran las cuentas de equipo del cliente de BranchCache que desea configurar.  
  
2.  En la Consola de administración de directivas de grupo, asegúrese de que esté seleccionada la opción **Objetos de directiva de grupo** y, en el panel de detalles, haga clic con el botón secundario en el objeto de directiva de grupo Equipos cliente de BranchCache creado anteriormente. Por ejemplo, si ha asignado al objeto de directiva de grupo el nombre Equipos cliente de BranchCache, haga clic con el botón secundario en **Equipos cliente de BranchCache**. Haga clic en **Editar**. Se abre la consola del Editor de administración de directivas de grupo.  
  
3.  En la consola Editor de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Configuración del equipo**, **directivas**, **Windows configuración**, **configuración de seguridad**, **Firewall de Windows con seguridad avanzada**, **Firewall de Windows con seguridad avanzada - LDAP**, **reglas de entrada**.  
  
4.  Haga clic con el botón secundario en **Reglas de entrada** y, a continuación, en **Nueva regla**. Se abre el Asistente para nueva regla de entrada.  
  
5.  En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: recuperación de contenido (usa HTTP)**. Haz clic en **Siguiente**.  
  
6.  En **Reglas predefinidas**, haga clic en **Siguiente**.  
  
7.  En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.  
  
    > [!IMPORTANT]  
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda recibir tráfico en este puerto.  
  
8.  Para crear la excepción de firewall para WS-Discovery, vuelva a hacer clic con el botón secundario en **Reglas de entrada** y, a continuación, haga clic en **Nueva regla**. Se abre el Asistente para nueva regla de entrada.  
  
9. En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: detección del mismo nivel (usa WSD)**. Haz clic en **Siguiente**.  
  
10. En **Reglas predefinidas**, haga clic en **Siguiente**.  
  
11. En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.  
  
    > [!IMPORTANT]  
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda recibir tráfico en este puerto.  
  
## <a name="bkmk_outbound"></a>Para configurar Firewall de Windows con reglas de tráfico saliente de seguridad avanzada  
  
1.  En la consola de Editor de administración de directivas de grupo, haga clic con el botón secundario en **Reglas de salida** y, a continuación, haga clic en **Nueva regla**. Se abre el Asistente para nueva regla de salida.  
  
2.  En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: recuperación de contenido (usa HTTP)**. Haz clic en **Siguiente**.  
  
3.  En **Reglas predefinidas**, haga clic en **Siguiente**.  
  
4.  En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.  
  
    > [!IMPORTANT]  
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda enviar tráfico en este puerto.  
  
5.  Para crear la excepción de firewall para WS-Discovery, vuelva a hacer clic con el botón secundario en **Reglas de salida** y, a continuación, haga clic en **Nueva regla**. Se abre el Asistente para nueva regla de salida.  
  
6.  En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: detección del mismo nivel (usa WSD)**. Haz clic en **Siguiente**.  
  
7.  En **Reglas predefinidas**, haga clic en **Siguiente**.  
  
8.  En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.  
  
    > [!IMPORTANT]  
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda enviar tráfico en este puerto.  
  


