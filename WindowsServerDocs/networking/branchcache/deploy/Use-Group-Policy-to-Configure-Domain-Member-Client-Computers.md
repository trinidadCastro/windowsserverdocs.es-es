---
title: Usar directiva de grupo para configurar equipos cliente miembros del dominio
description: Obtenga información acerca de cómo usar directiva de grupo para configurar equipos cliente pertenecientes a un dominio.
manager: dougkim
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: lizross
author: eross-msft
ms.date: 06/02/2018
ms.openlocfilehash: 281e67777ce54f1e03741f9d2120e0d4cafbb91f
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904659"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Usar directiva de grupo para configurar equipos cliente miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta sección, creará un objeto de directiva de grupo para todos los equipos de la organización, configurará los equipos cliente miembros del dominio con el modo caché distribuida o el modo caché hospedada, y configurará Firewall de Windows con seguridad avanzada para permitir el tráfico de BranchCache.

Esta sección contiene los siguientes procedimientos.

1.  [Para crear un objeto directiva de grupo y configurar los modos de BranchCache](#bkmk_gp)

2.  [Para configurar las reglas de tráfico de entrada de Firewall de Windows con seguridad avanzada](#bkmk_inbound)

3.  [Para configurar las reglas de tráfico de salida de Firewall de Windows con seguridad avanzada](#bkmk_outbound)

> [!TIP]
> En el procedimiento siguiente, se le indicará que cree un objeto de directiva de grupo en la Directiva de dominio predeterminada; sin embargo, puede crear el objeto en una unidad organizativa (OU) u otro contenedor que sea adecuado para su implementación.

Debe ser miembro de **Admins**. del dominio o equivalente para realizar estos procedimientos.

## <a name="to-create-a-group-policy-object-and-configure-branchcache-modes"></a><a name="bkmk_gp"></a>Para crear un objeto directiva de grupo y configurar los modos de BranchCache

1.  En un equipo en el que esté instalado el rol de servidor Active Directory Domain Services, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **Administración de directiva de grupo**. Se abre la Consola de administración de directivas de grupo.

2.  En la consola de administración de directiva de grupo, expanda la siguiente ruta de acceso: **bosque:** *example.com*, **dominios**, *example.com* **Directiva de grupo objetos**, donde *example.com* es el nombre del dominio donde se encuentran las cuentas de equipo cliente de BranchCache que desea configurar.

3.  Haga clic con el botón secundario en **Objetos de directiva de grupo** y, a continuación, haga clic en **Nuevo**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo (GPO). Por ejemplo, si desea asignar al objeto el nombre Equipos cliente de BranchCache, escriba **Equipos cliente de BranchCache**. Haga clic en **Aceptar**.

4.  En la Consola de administración de directivas de grupo, asegúrese de que esté seleccionada la opción **Objetos de directiva de grupo** y, en el panel de detalles, haga clic con el botón secundario en el objeto de directiva de grupo que acaba de crear. Por ejemplo, si ha asignado al objeto de directiva de grupo el nombre Equipos cliente de BranchCache, haga clic con el botón secundario en **Equipos cliente de BranchCache**. Haga clic en **Editar**. Se abre la consola de Editor de administración de directivas de grupo.

5.  En la consola de Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **plantillas administrativas: definiciones de directivas (archivos ADMX) recuperadas del equipo local**, **red**, **BranchCache**.

6.  Haga clic en **BranchCache** y, a continuación, en el panel de detalles, haga doble clic en **Activar BranchCache**. Se abre el cuadro de diálogo Configuración de directiva.

7.  En el cuadro de diálogo **Activar BranchCache**, haga clic en **Habilitado** y, a continuación, en **Aceptar**.

8.  Para habilitar el modo caché distribuida de BranchCache, en el panel de detalles, haga doble clic en **establecer el modo caché distribuida de BranchCache**. Se abre el cuadro de diálogo Configuración de directiva.

9. En el cuadro de diálogo **Establecer el modo Caché distribuida de BranchCache**, haga clic en **Habilitado** y, a continuación, haga clic en **Aceptar**.

10. Si tiene una o más sucursales donde va a implementar BranchCache en modo caché hospedada y ha implementado servidores de caché hospedada en esas oficinas, haga doble clic en **Habilitar detección automática de caché hospedada por punto de conexión de servicio**. Se abre el cuadro de diálogo Configuración de directiva.

11. En el cuadro de diálogo **Habilitar detección automática de caché hospedada por punto de conexión de servicio** , haga clic en **habilitado** y, a continuación, haga clic en **Aceptar**.

    > [!NOTE]
    > Cuando se habilita la configuración de directiva **establecer caché distribuida de BranchCache** y **Habilitar detección de caché hospedada automática por punto de conexión de servicio** , los equipos cliente operan en el modo caché distribuida de BranchCache a menos que encuentren un servidor de caché hospedada en la sucursal, momento en el que operan en modo caché hospedada.

12. Use los procedimientos siguientes para configurar las opciones de firewall en los equipos cliente mediante directiva de grupo.

## <a name="to-configure-windows-firewall-with-advanced-security-inbound-traffic-rules"></a><a name="bkmk_inbound"></a>Para configurar las reglas de tráfico de entrada de Firewall de Windows con seguridad avanzada

1.  En la consola de administración de directiva de grupo, expanda la siguiente ruta de acceso: **bosque:** *example.com*, **dominios**, *example.com* **Directiva de grupo objetos**, donde *example.com* es el nombre del dominio donde se encuentran las cuentas de equipo cliente de BranchCache que desea configurar.

2.  En la Consola de administración de directivas de grupo, asegúrese de que esté seleccionada la opción **Objetos de directiva de grupo** y, en el panel de detalles, haga clic con el botón secundario en el objeto de directiva de grupo Equipos cliente de BranchCache creado anteriormente. Por ejemplo, si ha asignado al objeto de directiva de grupo el nombre Equipos cliente de BranchCache, haga clic con el botón secundario en **Equipos cliente de BranchCache**. Haga clic en **Editar**. Se abre la consola de Editor de administración de directivas de grupo.

3.  En la consola de Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **configuración de Windows**, configuración de **seguridad**, **firewall de Windows con seguridad avanzada**, **firewall de Windows con seguridad avanzada-LDAP**, **reglas de entrada**.

4.  Haga clic con el botón secundario en **Reglas de entrada** y, a continuación, en **Nueva regla**. Se abre el Asistente para nueva regla de entrada.

5.  En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: recuperación de contenido (utiliza http)**. Haga clic en **Next**.

6.  En **Reglas predefinidas**, haga clic en **Siguiente**.

7.  En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.

    > [!IMPORTANT]
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda recibir tráfico en este puerto.

8.  Para crear la excepción de firewall para WS-Discovery, vuelva a hacer clic con el botón secundario en **Reglas de entrada** y, a continuación, haga clic en **Nueva regla**. Se abre el Asistente para nueva regla de entrada.

9. En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: detección de pares (usa WSD)**. Haga clic en **Next**.

10. En **Reglas predefinidas**, haga clic en **Siguiente**.

11. En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.

    > [!IMPORTANT]
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda recibir tráfico en este puerto.

## <a name="to-configure-windows-firewall-with-advanced-security-outbound-traffic-rules"></a><a name="bkmk_outbound"></a>Para configurar las reglas de tráfico de salida de Firewall de Windows con seguridad avanzada

1.  En la consola de Editor de administración de directivas de grupo, haga clic con el botón secundario en **Reglas de salida** y, a continuación, haga clic en **Nueva regla**. Se abre el Asistente para nueva regla de salida.

2.  En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: recuperación de contenido (utiliza http)**. Haga clic en **Next**.

3.  En **Reglas predefinidas**, haga clic en **Siguiente**.

4.  En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.

    > [!IMPORTANT]
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda enviar tráfico en este puerto.

5.  Para crear la excepción de firewall para WS-Discovery, vuelva a hacer clic con el botón secundario en **Reglas de salida** y, a continuación, haga clic en **Nueva regla**. Se abre el Asistente para nueva regla de salida.

6.  En **tipo de regla**, haga clic en **predefinida**, expanda la lista de opciones y, a continuación, haga clic en **BranchCache: detección de pares (usa WSD)**. Haga clic en **Next**.

7.  En **Reglas predefinidas**, haga clic en **Siguiente**.

8.  En **Acción**, asegúrese de que esté seleccionada la opción **Permitir la conexión** y, a continuación, haga clic en **Finalizar**.

    > [!IMPORTANT]
    > Debe seleccionar la opción **Permitir la conexión** para que el cliente de BranchCache pueda enviar tráfico en este puerto.



