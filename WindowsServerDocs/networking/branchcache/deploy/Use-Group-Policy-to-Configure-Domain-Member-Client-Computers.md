---
title: Usar la directiva de grupo para configurar los equipos de cliente de miembro de dominio
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 96bf84df14eac8016a898e92eb96f90435c53c98
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Usar la directiva de grupo para configurar los equipos de cliente de miembro de dominio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar estos procedimientos para crear un objeto de directiva de grupo para todos los equipos de la organización, para configurar equipos cliente miembros del dominio con el modo de caché distribuida o memoria caché hospedada y para configurar el Firewall de Windows con seguridad avanzada para permitir el tráfico BranchCache.  
  
Esta sección contiene los siguientes procedimientos.  
  
1.  [Para crear un objeto de directiva de grupo y configurar BranchCache modos](#bkmk_gp)  
  
2.  [Para configurar Windows Firewall con reglas de tráfico entrante de seguridad de avanzada](#bkmk_inbound)  
  
3.  [Para configurar Windows Firewall con reglas de tráfico saliente de seguridad avanzada](#bkmk_outbound)  
  
> [!TIP]  
> En el siguiente procedimiento, que se indique para crear un objeto de directiva de grupo en la directiva de dominio predeterminada, pero puedes crear el objeto en una unidad organizativa (OU) o en otro contenedor que es apropiado para la implementación.  
  
Debe ser miembro del **administradores de dominio**, o equivalente para realizar estos procedimientos.  
  
## <a name="bkmk_gp"></a>Para crear un objeto de directiva de grupo y configurar BranchCache modos  
  
1.  En un equipo en el que está instalado el rol de servidor de servicios de dominio de Active Directory, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Group Policy Management**. Abre la consola de administración de directivas de grupo.  
  
2.  En la consola de administración de directivas de grupo, expanda la siguiente ruta de acceso: **bosque:** *ejemplo.com*, **dominios**, *ejemplo.com*, **objetos de directiva de grupo**, donde *ejemplo.com* es el nombre del dominio donde se encuentran las cuentas de equipo del cliente BranchCache que quieras configurar.  
  
3.  Haz clic en **objetos de directiva de grupo**y, a continuación, haz clic en **nueva**. La **nuevo GPO** abre el cuadro de diálogo. En **nombre**, escribe un nombre para el nuevo objeto de directiva de grupo (GPO). Por ejemplo, si quieres nombrar el objeto BranchCache los equipos cliente, escribe **los equipos cliente BranchCache**. Haz clic en **Aceptar**.  
  
4.  En la consola de administración de directivas de grupo, asegúrate de que **objetos de directiva de grupo** está seleccionado y, en el panel de detalles, haz clic en el GPO que acabas de crear. Por ejemplo, si los equipos de cliente de GPO BranchCache el nombre, haz clic en **los equipos cliente BranchCache**. Haz clic en **editar**. Abre la consola del Editor de administración de directivas de grupo.  
  
5.  En la consola del Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **plantillas administrativas: definiciones de directivas (archivos ADMX) recuperan desde el equipo local**, **red**, **BranchCache**.  
  
6.  Haz clic en **BranchCache**y, a continuación, en el panel de detalles, haz doble clic en **activar BranchCache**. Abre el cuadro de diálogo de configuración de directiva.  
  
7.  En la **activar BranchCache** cuadro de diálogo, haz clic en **habilitado**y, a continuación, haz clic en **Aceptar**.  
  
8.  Para habilitar el modo de caché distribuida BranchCache, en el panel de detalles, haz doble clic en **modo de caché distribuida BranchCache establecer**. Abre el cuadro de diálogo de configuración de directiva.  
  
9. En la **modo de establecer BranchCache distribuido caché** cuadro de diálogo, haz clic en **habilitado**y, a continuación, haz clic en **Aceptar**.  
  
10. Si tienes uno o más sucursales donde vas a implementar BranchCache en modo de caché hospedada y ha implementado hospedados en las oficinas de los servidores de caché, haz doble clic en **habilitar hospedadas caché la detección automática al punto de conexión de servicio**. Abre el cuadro de diálogo de configuración de directiva.  
  
11. En la **habilitar hospedadas caché la detección automática al punto de conexión de servicio** cuadro de diálogo, haz clic en **habilitado**y, a continuación, haz clic en **Aceptar**.  
  
    > [!NOTE]  
    > Al habilitar ambas la **modo de caché distribuida BranchCache establecer** y **habilitar hospedadas caché la detección automática al punto de conexión de servicio** configuración de directiva, los equipos cliente funcionan en el modo de caché distribuida de BranchCache a menos que se encuentran un servidor de la memoria caché hospedada en las sucursales, momento en que trabajan en modo de caché hospedada.  
  
12. Usa los siguientes procedimientos para configurar las opciones de firewall en los equipos cliente mediante la directiva de grupo.  
  
## <a name="bkmk_inbound"></a>Para configurar Windows Firewall con reglas de tráfico entrante de seguridad de avanzada  
  
1.  En la consola de administración de directivas de grupo, expanda la siguiente ruta de acceso: **bosque:** *ejemplo.com*, **dominios**, *ejemplo.com*, **objetos de directiva de grupo**, donde *ejemplo.com* es el nombre del dominio donde se encuentran las cuentas de equipo del cliente BranchCache que quieras configurar.  
  
2.  En la consola de administración de directivas de grupo, asegúrate de que **objetos de directiva de grupo** está seleccionado y, en el panel de detalles, haz clic en los equipos de cliente BranchCache GPO que creaste anteriormente. Por ejemplo, si los equipos de cliente de GPO BranchCache el nombre, haz clic en **los equipos cliente BranchCache**. Haz clic en **editar**. Abre la consola del Editor de administración de directivas de grupo.  
  
3.  En la consola del Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **configuración de Windows**, **la configuración de seguridad**, **Firewall de Windows con seguridad avanzada**, **Firewall de Windows con seguridad avanzada: LDAP**, **reglas de entrada**.  
  
4.  Haz clic en **reglas de entrada**y, a continuación, haz clic en **nueva regla**. Abre el Asistente para nueva regla de entrada.  
  
5.  En **tipo de regla**, haz clic en **predefinida**, expande la lista de opciones y, a continuación, haz clic en **BranchCache: recuperación de contenido (usa HTTP)**. Haz clic en **siguiente**.  
  
6.  En **reglas predefinidas**, haz clic en **siguiente**.  
  
7.  En **acción**, asegúrate de que **permitir la conexión** está seleccionado y, a continuación, haz clic en **finalizar**.  
  
    > [!IMPORTANT]  
    > Debes seleccionar **permitir la conexión** para el cliente BranchCache poder recibir tráfico en este puerto.  
  
8.  Para crear la excepción de firewall del detección WS, vuelve a secundario **reglas de entrada**y, a continuación, haz clic en **nueva regla**. Abre el Asistente para nueva regla de entrada.  
  
9. En **tipo de regla**, haz clic en **predefinida**, expande la lista de opciones y, a continuación, haz clic en **BranchCache: detección de sistema del mismo nivel (usa WSD)**. Haz clic en **siguiente**.  
  
10. En **reglas predefinidas**, haz clic en **siguiente**.  
  
11. En **acción**, asegúrate de que **permitir la conexión** está seleccionado y, a continuación, haz clic en **finalizar**.  
  
    > [!IMPORTANT]  
    > Debes seleccionar **permitir la conexión** para el cliente BranchCache poder recibir tráfico en este puerto.  
  
## <a name="bkmk_outbound"></a>Para configurar Windows Firewall con reglas de tráfico saliente de seguridad avanzada  
  
1.  En la consola del Editor de administración de directivas de grupo, haz clic en **reglas de salida**y, a continuación, haz clic en **nueva regla**. Abre el Asistente para nueva regla de salida.  
  
2.  En **tipo de regla**, haz clic en **predefinida**, expande la lista de opciones y, a continuación, haz clic en **BranchCache: recuperación de contenido (usa HTTP)**. Haz clic en **siguiente**.  
  
3.  En **reglas predefinidas**, haz clic en **siguiente**.  
  
4.  En **acción**, asegúrate de que **permitir la conexión** está seleccionado y, a continuación, haz clic en **finalizar**.  
  
    > [!IMPORTANT]  
    > Debes seleccionar **permitir la conexión** para el cliente BranchCache poder enviar el tráfico en este puerto.  
  
5.  Para crear la excepción de firewall del detección WS, vuelve a secundario **reglas de salida**y, a continuación, haz clic en **nueva regla**. Abre el Asistente para nueva regla de salida.  
  
6.  En **tipo de regla**, haz clic en **predefinida**, expande la lista de opciones y, a continuación, haz clic en **BranchCache: detección de sistema del mismo nivel (usa WSD)**. Haz clic en **siguiente**.  
  
7.  En **reglas predefinidas**, haz clic en **siguiente**.  
  
8.  En **acción**, asegúrate de que **permitir la conexión** está seleccionado y, a continuación, haz clic en **finalizar**.  
  
    > [!IMPORTANT]  
    > Debes seleccionar **permitir la conexión** para el cliente BranchCache poder enviar el tráfico en este puerto.  
  


