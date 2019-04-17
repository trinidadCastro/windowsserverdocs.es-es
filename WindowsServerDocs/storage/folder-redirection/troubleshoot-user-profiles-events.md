---
title: Solución de problemas de los perfiles de usuario con eventos
description: Cómo solucionar problemas al cargar y descargar los perfiles de usuario mediante el uso de eventos y registros de seguimiento.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082685"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solución de problemas de los perfiles de usuario con eventos

>Se aplica a: 10 de Windows, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.

En este tema se describe cómo solucionar problemas al cargar y descargar los perfiles de usuario mediante el uso de eventos y registros de seguimiento. Las secciones siguientes describen cómo usar los tres registros de eventos que registrar la información de perfil de usuario.

## <a name="step-1-checking-events-in-the-application-log"></a>Paso 1: Comprobación de eventos en el registro de aplicaciones

El primer paso para solucionar los problemas de carga y descarga de perfiles (incluidos los perfiles de usuario móvil) consiste en usar el Visor de eventos para examinar los eventos de Error y advertencia que inicie sesión registros del servicio de perfiles de usuario en la aplicación de usuario.

Aquí es cómo ver los eventos de servicios de perfiles de usuario en el registro de aplicación:

1. Iniciar el Visor de eventos. Para ello, abra el **Panel de Control**, seleccione **sistema y seguridad**y, a continuación, en la sección **Herramientas administrativas** , seleccione **Ver registros de sucesos**. Se abre la ventana Visor de eventos.
2. En el árbol de consola, en primer lugar vaya a **Registros de Windows**, a continuación, la **aplicación**.
3. En el panel Acciones, seleccione **Filtrar registro actual**. Se abre el cuadro de diálogo Filtrar registro actual.
4. En el cuadro **orígenes de eventos** , seleccione la casilla de verificación **Servicio de perfiles de usuario** y, a continuación, seleccione **Aceptar**.
5. Revise la lista de sucesos, preste especial atención a los eventos de Error.
6. Cuando encuentre notables eventos, seleccione el vínculo de ayuda en línea de registro de eventos para mostrar información adicional y los procedimientos de solución de problemas.
7. Para realizar la solución de problemas adicional, tenga en cuenta la fecha y hora de eventos notables y, a continuación, examine el registro de operativas (tal como se describe en el paso 2) para ver los detalles acerca de lo que el servicio de perfiles de usuario hacía alrededor de la hora de los eventos de Error o advertencia.

>[!NOTE]
>Puede omitir sin problemas el servicio de perfiles de usuario evento 1530 "Windows detecta que el archivo del registro todavía está en uso por otras aplicaciones o servicios."

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Paso 2: Ver el registro de operativas para el servicio de perfiles de usuario

Si no se puede resolver el problema mediante el registro de aplicación por sí solo, use el procedimiento siguiente para ver los eventos de servicio de perfiles de usuario en el registro de operativas. Este registro muestra algunos de los mecanismos internos del servicio y puede ayudar a determinar dónde en la carga de perfil o descargar proceso que se está produciendo el problema.

El registro de aplicación de Windows y el registro de servicio de perfiles de usuario operativas están habilitadas de forma predeterminada en todas las instalaciones de Windows.

Aquí es cómo ver el registro de operativas para el servicio de perfiles de usuario:

1. En caso de Visor de árbol de consola, desplácese a **Servicios de registros de aplicaciones y**, a continuación, **Microsoft**, a continuación, **Windows**, a continuación, **Servicio de perfiles de usuario**y, a continuación, **operativa**.
2. Examine los eventos que se ha producido alrededor de la hora de los eventos de Error o advertencia que anotó en el registro de la aplicación.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Paso 3: Habilitar y ver analíticas y registros de depuración

Si necesita más detalle del que proporciona el registro operativo, puede habilitar analíticos y depurar los registros en el equipo afectado. Este nivel de registro es mucho más detallado y debe deshabilitarse excepto cuando intenta solucionar un problema.

Aquí es cómo habilitar y ver analíticas y depurar los registros:

1. En el panel de **acciones** del Visor de eventos, seleccione **la vista**y, a continuación, seleccione **Mostrar Depurar registros analíticos y**.
2. Vaya a **registros de aplicaciones y servicios**, a continuación, **Microsoft**, a continuación, **Windows**, a continuación, **servicio de perfiles de usuario**y, a continuación, **diagnóstico**.
3. Seleccione **Habilitar registro** y, a continuación, seleccione **Sí**. Esto permite el registro de diagnóstico, que se iniciará el registro.
4. Si necesita información incluso más detallada, vea [paso 4: creación y descodificación un seguimiento de](#step-4:-creating-and-decoding-a-trace) para obtener más información acerca de cómo crear un registro de seguimiento.
5. Cuando haya terminado de solucionar el problema, navegue hasta el registro de **diagnóstico** , seleccione **Deshabilitar el registro**, seleccione la **vista** y, a continuación, desactive la casilla de verificación **Mostrar Depurar registros analíticos y** para ocultar analíticos y registro de depuración.

## <a name="step-4-creating-and-decoding-a-trace"></a>Paso 4: Creación y descodificación un seguimiento

Si no se puede resolver el problema de uso de eventos, puede crear un registro de seguimiento (un archivo ETL) al reproducir el problema y, a continuación, descodificar mediante símbolos públicos desde el servidor de símbolos de Microsoft. Registros de seguimiento proporcionan información muy específica acerca de qué está haciendo el servicio de perfiles de usuario y puede ayudarle a identificar donde se produjo el error.

Es la mejor estrategia cuando se usa el seguimiento de ETL capturar en primer lugar el registro más pequeño posible. Una vez que se descodifica el registro, busque en el registro de errores.

Aquí es cómo crear y descodificar un seguimiento para el servicio de perfiles de usuario:

1. Inicie sesión el equipo donde el usuario está experimentando problemas, con una cuenta que sea miembro del grupo de administradores locales.
2. Desde un símbolo del sistema con privilegios elevados, escriba los siguientes comandos, donde *\ < ruta >* es la ruta de acceso a una carpeta local que ha creado anteriormente, por ejemplo C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. En la pantalla de inicio, seleccione el nombre de usuario y, a continuación, seleccione **cuenta de modificador**, tenga cuidado de no cierre el Administrador de la sesión. Si está utilizando Escritorio remoto, cierre la sesión de administrador para establecer la sesión de usuario.
4. Reproducir el problema. El procedimiento para reproducir el problema suele iniciar la sesión como el usuario que observó el problema, inicie sesión el usuario desactivado, o ambos.
5. Después de reproducir el problema, inicie sesión como administrador local nuevo.
6. Desde un símbolo del sistema con privilegios elevados, ejecute el siguiente comando para guardar el registro en un archivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Escriba el siguiente comando para exportar el archivo ETL en un archivo legible en el directorio actual (es probable que la carpeta principal o la carpeta %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra el archivo **Summary.txt** y el archivo **Dumpfile.xml** (puede abrirlos en Microsoft Excel para ver más fácilmente los detalles completos del registro). Busque eventos que incluyen **producirá un error** o **no se pudo**; puede pasar por alto las líneas que incluyen el nombre de evento **desconocido** .

## <a name="more-information"></a>Más información

* [Implementar perfiles de usuario móvil](deploy-roaming-user-profiles.md)