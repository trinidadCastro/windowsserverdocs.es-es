---
title: Solución de problemas de perfiles de usuario con eventos
description: Cómo solucionar problemas al cargar y descargar perfiles de usuario mediante eventos y registros de seguimiento.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394382"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solución de problemas de perfiles de usuario con eventos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server (canal semianual).

En este tema se trata cómo solucionar problemas al cargar y descargar perfiles de usuario mediante eventos y registros de seguimiento. En las secciones siguientes se describe cómo usar los tres registros de eventos que registran información de perfil de usuario.

## <a name="step-1-checking-events-in-the-application-log"></a>Paso 1: Comprobar eventos en el registro de aplicaciones

El primer paso para la solución de problemas con la carga y descarga de perfiles de usuario (incluidos los perfiles de usuario móvil) es usar Visor de eventos para examinar los eventos de advertencia y error que registra el servicio de perfiles de usuario en el registro de aplicaciones.

Aquí se muestra cómo ver eventos de servicios de perfiles de usuario en el registro de aplicaciones:

1. Inicia el Visor de eventos. Para ello, abre el **Panel de control**, selecciona **Sistema y seguridad** y, a continuación, en la sección **Herramientas administrativas**, selecciona **Ver registros de eventos**. Se abre la ventana Visor de eventos.
2. En el árbol de consola, ve primero a **Registros de Windows** y después a **Aplicación**.
3. En el panel Acciones, haz clic en **Filtrar registro actual**. Se abre el cuadro de diálogo Filtrar registro actual.
4. En el cuadro **Orígenes de eventos**, activa la casilla **Servicio de perfiles de usuario** y, a continuación, selecciona **Aceptar**.
5. Revisa la lista de eventos y presta especial atención a los eventos de error.
6. Si encuentras eventos destacados, selecciona el vínculo Ayuda en pantalla de Registro de eventos para mostrar información adicional y procedimientos de solución de problemas.
7. Para realizar más tareas de solución de problemas, ten en cuenta la fecha y la hora de los eventos destacados y examina el registro operativo (como se describe en el paso 2) para ver los detalles sobre qué estaba haciendo el servicio de perfiles de usuario cuando se produjeron los eventos de error o de advertencia.

>[!NOTE]
>Puedes omitir con seguridad el evento del servicio de perfiles de usuario 1530 "Windows detectó que otras aplicaciones o servicios siguen usando el archivo de Registro".

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Paso 2: Ver el registro operativo del servicio de perfiles de usuario

Si no puedes resolver el problema solo con el registro de aplicaciones, utiliza el procedimiento siguiente para ver los eventos del servicio de perfiles de usuario en el registro operativo. Este registro muestra algunos de los trabajos internos del servicio y puede ayudar a identificar dónde se está produciendo el problema en el proceso de carga o descarga de perfiles.

El registro de aplicaciones de Windows y el registro operativo de servicio de perfiles de usuario están habilitados de forma predeterminada en todas las instalaciones de Windows.

A continuación se explica cómo ver el registro operativo del servicio de perfiles de usuario:

1. En el árbol de consola del Visor de eventos, ve a **Registros de aplicaciones y servicios**, **Microsoft**, **Windows**, **Servicio de perfiles de usuario** y, a continuación, **Operativo**.
2. Examine los eventos que se produjeron aproximadamente a la hora de los eventos de error o de advertencia que anotó en el registro de aplicaciones.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Paso 3: Habilitar y ver los registros analíticos y de depuración

Si necesitas más detalles de los proporciona el registro operativo, puedes habilitar los registros analíticos y de depuración en el equipo afectado. Este nivel de registro es mucho más detallado y debe deshabilitarse, excepto cuando se intenta solucionar un problema.

A continuación se explica cómo habilitar y ver los registros analíticos y de depuración:

1. En el panel **Acciones** del Visor de eventos, selecciona **Ver** y, a continuación, selecciona **Mostrar registros analíticos y de depuración**.
2. Dirígete a **Registros de aplicaciones y servicios**, **Microsoft**, **Windows**, **Servicio de perfiles de usuario** y, a continuación, **Operativo**.
3. Selecciona **Habilitar registro** y después selecciona **Sí**. Esta acción habilita el registro de diagnóstico, que iniciará el registro.
4. Si necesitas información aún más detallada, consulta el [Paso 4: Crear y descodificar un seguimiento](#step-4-creating-and-decoding-a-trace) para obtener más información acerca de cómo crear un registro de seguimiento.
5. Una vez resuelto el problema, ve al registro de **Diagnóstico**, selecciona **Deshabilitar registro**, selecciona **Ver** y, a continuación, desactiva la casilla **Mostrar registros analíticos y de depuración** para ocultar el registro analítico y de depuración.

## <a name="step-4-creating-and-decoding-a-trace"></a>Paso 4: Crear y descodificar un seguimiento

Si no puedes resolver el problema mediante eventos, puedes crear un registro de seguimiento (un archivo ETL) mientras se reproduce el problema y, a continuación, descodificarlo con símbolos públicos del servidor de símbolos de Microsoft. Los registros de seguimiento proporcionan información muy específica sobre lo que hace el servicio de perfiles de usuario y pueden ayudar a identificar dónde se ha producido el error.

La mejor estrategia al usar el seguimiento ETL es capturar primero el registro más pequeño posible. Una vez descodificado el registro, busca errores en el registro.

A continuación se explica cómo crear y descodificar un seguimiento en el servicio de perfiles de usuario:

1. Inicia sesión en el equipo donde el usuario experimenta problemas con una cuenta que sea miembro del grupo Administradores local.
2. En un símbolo del sistema con privilegios elevados, escribe los siguientes comandos, donde *\<Path\>* es la ruta de acceso a una carpeta local que has creado anteriormente, por ejemplo, C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. En la pantalla Inicio, selecciona el nombre de usuario y, a continuación, elige **Cambiar cuenta**, con cuidado de no cerrar la sesión del administrador. Si estás utilizando el Escritorio remoto, cierra la sesión de administrador para establecer la sesión de usuario.
4. Reproduce el problema. El procedimiento para reproducir el problema suele ser iniciar sesión como el usuario que experimenta el problema, cerrar la sesión del usuario o ambas cosas.
5. Después de reproducir el problema, vuelve a iniciar sesión como administrador local.
6. Desde un símbolo del sistema con privilegios elevados, ejecuta el siguiente comando para guardar el registro en un archivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Escribe el siguiente comando para exportar el archivo ETL en un archivo legible en el directorio actual (probablemente la carpeta principal o la carpeta %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abre el archivo **Summary.txt** y el archivo **Dumpfile.xml** (puedes abrirlos en Microsoft Excel para ver más fácilmente los detalles completos del registro). Busca eventos que incluyan **fail** o **failed**; puedes omitir con seguridad las líneas que incluyan el nombre de evento **Unknown**.

## <a name="more-information"></a>Información adicional

* [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)