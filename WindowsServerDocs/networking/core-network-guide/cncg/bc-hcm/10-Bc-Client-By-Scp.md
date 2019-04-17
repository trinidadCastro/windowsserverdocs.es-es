---
title: Configurar cliente la detección automática de memoria caché hospedada por punto de conexión de servicio
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b12fa6f9e11c8816d74c9013dd80b3fa38d0a478
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar cliente la detección automática de memoria caché hospedada por punto de conexión de servicio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con este procedimiento, puedes usar directivas de grupo para habilitar y configurar el modo de caché BranchCache hospedado en equipos unidos a un domain\ que se ejecutan los siguientes sistemas operativos de Windows compatibles con BranchCache\.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Para configurar los equipos unidos a un dominio que ejecutan Windows Server 2008 R2 o Windows 7, consulta el tema de Windows Server 2008 R2 [Guía de implementación de BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Usar la directiva de grupo para configurar a los clientes para el modo de la memoria caché hospedada

1. En un equipo en el que está instalado el rol de servidor de servicios de dominio de Active Directory, abre el administrador del servidor, seleccionar el servidor Local, haz clic en **herramientas**y, a continuación, haz clic en **Group Policy Management**. Abre la consola de administración de directivas de grupo.

2. En la consola de administración de directivas de grupo, expanda la siguiente ruta de acceso: **bosque:***corp.contoso.com*, **dominios**, *corp.contoso.com*, **objetos de directiva de grupo**, donde *corp.contoso.com* es el nombre del dominio donde se encuentran las cuentas de equipo del cliente BranchCache que quieras configurar.

3. Clic Right\ **objetos de directiva de grupo**y, a continuación, haz clic en **nueva**. La **nuevo GPO** abre el cuadro de diálogo. En **nombre**, escribe un nombre para la nueva directiva de grupo objeto \(GPO\). Por ejemplo, si quieres nombrar el objeto BranchCache los equipos cliente, escribe **los equipos cliente BranchCache**. Haz clic en **Aceptar**.

4. En la consola de administración de directivas de grupo, asegúrate de que **objetos de directiva de grupo** están seleccionado y en los detalles panel right\-haga clic en el GPO que has creado. Por ejemplo, si denominado los equipos de cliente BranchCache de GPO, right\ clic **los equipos cliente BranchCache**. Haz clic en **editar**. Abre la consola del Editor de administración de directivas de grupo.

5. En la consola del Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **plantillas administrativas: directiva definiciones \(ADMX files\) recuperado desde el equipo local**, **red**, **BranchCache**.

6. Haz clic en **BranchCache**y, después, en el panel de detalles, double\ clic **activar BranchCache**. La **activar BranchCache** abre el cuadro de diálogo.
  
7.  En la **activar BranchCache** cuadro de diálogo, haz clic en **habilitado**y, a continuación, haz clic en **Aceptar**.

8. En la consola del Editor de administración de directivas de grupo, asegúrate de que **BranchCache** sigue estando seleccionado y, a continuación, en el panel de detalles double\ clic **habilitar hospedadas caché la detección automática al punto de conexión de servicio**. Abre el cuadro de diálogo de configuración de directiva.

9. En la **habilitar hospedadas caché la detección automática al punto de conexión de servicio** cuadro de diálogo, haz clic en **habilitado**y, a continuación, haz clic en **Aceptar**.

10. Para permitir que los equipos cliente descargar y almacenar en caché contenido de BranchCache servidores server\ contenidos del archivo: en la consola del Editor de administración de directivas de grupo, asegúrate de que **BranchCache** sigue estando seleccionado y, a continuación, en el panel de detalles double\ clic **BranchCache para archivos de red**. La **configurar BranchCache para archivos de red** abre el cuadro de diálogo. 
11. En la **configurar BranchCache para archivos de red** cuadro de diálogo, haz clic en **habilitado**. En **opciones**, escribe un valor numérico, en milisegundos, para el tiempo de latencia de red máximo ida y vuelta y, a continuación, haz clic en **Aceptar**.
  
    > [!NOTE]
    > De manera predeterminada, los equipos cliente almacena en caché contenido de los servidores de archivos si la latencia de red de ida y vuelta tiene más de 80 milisegundos.
  
12. Para configurar la cantidad de espacio de disco duro asignado en cada equipo cliente para la caché BranchCache: en la consola del Editor de administración de directivas de grupo, asegúrate de que **BranchCache** sigue estando seleccionado y, a continuación, en el panel de detalles double\ clic **establecer el porcentaje de espacio en disco usado para la memoria caché del equipo cliente**. La **establecer el porcentaje de espacio en disco usado para la memoria caché del equipo cliente** abre el cuadro de diálogo. Haz clic en **habilitado**y, a continuación, en **opciones** escribe un valor numérico que representa el porcentaje de espacio en disco utilizado en cada equipo cliente para la caché BranchCache. Haz clic en **Aceptar**.

13. Para especificar la antigüedad de forma predeterminada, en días, para que los segmentos son válidos en la memoria caché de datos de BranchCache en los equipos cliente: en la consola del Editor de administración de directivas de grupo, asegúrate de que **BranchCache** sigue estando seleccionado y, a continuación, en el panel de detalles double\ clic **establece la edad de los segmentos en la memoria caché de datos**. La **establece la edad de los segmentos en la memoria caché de datos** abre el cuadro de diálogo. Haz clic en **habilitado**y, a continuación, en el panel de detalles, escribe el número de días que prefieras. Haz clic en **Aceptar**.

14. Configurar la configuración de directiva BranchCache adicional para los equipos cliente según corresponda para su implementación.

15. Actualizar la directiva de grupo en los equipos de cliente de office rama ejecutando el comando **comando gpupdate/force**, o los equipos cliente se reinició.

La implementación de modo de la memoria caché hospedada de BranchCache ahora está completa.

Para obtener más información sobre las tecnologías en esta guía, consulte [recursos adicionales](11-Bc-Hcm-additional-resources.md).