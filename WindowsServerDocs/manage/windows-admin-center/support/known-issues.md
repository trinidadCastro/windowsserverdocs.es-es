---
title: Problemas conocidos de Windows Admin Center
description: Problemas conocidos de Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: b0159d88251c7f4f6422dffd8f1e1414e1f30f33
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297110"
---
# Problemas conocidos de Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Si encuentras un problema no descrito en esta página, [háznoslo saber](http://aka.ms/WACfeedback).

## Integrador de Lenovo XClarity
Existe una incompatibilidad entre una combinación de la versión específica de Lenovo XClarity integrador y Windows Admin Center. Si actualmente usa o tienes previsto usar la extensión de Lenovo XClarity integrador de Windows Admin Center, aquí es lo que necesitas saber:

- Lenovo XClarity integrador versión de extensión 1.0.4 es totalmente compatible con Windows Admin Center 1809.5.
- Lenovo XClarity integrador versión de extensión 1.0.4 se expone un problema de compatibilidad en Windows Admin Center 1904. Los ingenieros de Microsoft y Lenovo están investigando activamente juntos y esperamos proporcionar una solución tan pronto como sea posible. Las actualizaciones se registrará aquí en el sitio de documentos de Windows Admin Center y que también puede hacer referencia a la [página de soporte técnico de Lenovo](https://support.lenovo.com/solutions/ht507549) como referencia.
- Si eres un usuario de la funcionalidad de Lenovo XClarity frecuentes, puede mantener en Windows Admin Center 1809.5 para seguir usando XClarity integrador 1.0.4 o puede actualizar a Windows Admin Center 1904 y por separado, usar el Administrador de XClarity independiente software como solución por ahora.


## Instalador

- Al instalar Windows Admin Center con su propio certificado, ten en cuenta que si copias la huella digital desde la herramienta MMC del Administrador de certificados, [contendrá un carácter no válido al principio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como una solución alternativa, escribe el primer carácter de la huella digital y copiar/pega el resto.

- No se admite el uso del puerto inferior a 1024. En el modo de servicio, puede configurar opcionalmente el puerto 80 para redirigir el puerto especificado.

- Si se detiene y se deshabilita el servicio Windows Update (wuauserv), se producirá un error en el programa de instalación. [19100629]

### Actualizar versión

- Al actualizar Windows Admin Center en modo de servicio desde una versión anterior, si usas msiexec en modo silencioso, pueden producirse un problema donde se elimina la regla de firewall de entrada para el puerto de Windows Admin Center.
  - Para volver a crear la regla, ejecuta el comando siguiente desde una consola de PowerShell con privilegios elevados, sustituyendo \<port> con el puerto configurado para Windows Admin Center (predeterminado 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## General

- Si tienes instalado como una puerta de enlace en **Windows Server 2016** en un uso intensivo de Windows Admin Center, el servicio puede bloquearse con un error en el registro de eventos que contiene ```Faulting application name: sme.exe``` y ```Faulting module name: WsmSvc.dll```. Esto es debido a un error que se ha solucionado en Windows Server 2019. La revisión de Windows Server 2016 se incluyó la de febrero de 2019 acumulativa actualizar, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si tienes Windows Admin Center instalado como una puerta de enlace y la lista de conexión parece estar dañado, siga estos pasos:

>[!WARNING]
>Esta acción eliminará la lista de conexión y la configuración para todos los usuarios de Windows Admin Center en la puerta de enlace.

  1. Desinstala Windows Admin Center
  2. Elimina la carpeta **Server Management Experience** en **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Volver a instalar Windows Admin Center

- Si dejas la herramienta abierta e inactivo durante un largo período de tiempo, es posible que recibas varios errores tipo **Error: el estado de espacio de ejecución no es válido para esta operación**. Si esto ocurre, actualiza el explorador. Si encuentras esto, [envíanos tus comentarios](http://aka.ms/WACfeedback).

- Es posible que encuentres un **Error 500** al actualizar páginas con direcciones URL muy largas. [12443710]

- En algunas herramientas, el corrector ortográfico del explorador puede marcar ciertos valores de campo como errores ortográficos. [12425477]

- En algunas herramientas, los botones de comando no pueden reflejar los cambios de estado inmediatamente después de hacer clic y la herramienta de la interfaz de usuario puede que no refleje automáticamente los cambios realizados en determinadas propiedades. Puedes hacer clic en **Actualizar** para recuperar el estado más reciente desde el servidor de destino. [11445790]

- Filtrado de etiqueta en la lista de conexiones - si selecciona las conexiones con las casillas de verificación de selección múltiple, filtrar la lista de conexión por etiquetas, la selección original persiste por lo que cualquier acción que selecciones se aplicará a todas las máquinas seleccionadas anteriormente. [18099259]

- Puede haber una variación menor entre los números de versión de sistemas operativos que se ejecutan en módulos de Windows Admin Center y lo que aparece en el aviso de Software de terceros 3.

### Administrador de extensiones

- Al actualizar Windows Admin Center, debe volver a instalar las extensiones.
- Si agregas una extensión de fuentes que no está accesible, no hay ninguna advertencia. [14412861]

## Problemas específicos del explorador

### Microsoft Edge

- En algunos casos, puedes encontrar tiempos de carga largos al utilizar Microsoft Edge para acceder a una puerta de enlace de Windows Admin Center a través de Internet. Esto puede ocurrir en VM de Azure donde la puerta de enlace de Windows Admin Center está usando un certificado autofirmado. [13819912]

- Al utilizar Azure Active Directory como proveedor de identidades y Windows Admin Center se configura con un certificado autofirmado o, en caso contrario , no de confianza, no puedes completar la autenticación de AAD en Microsoft Edge.  [15968377]

- Si tienes Windows Admin Center implementado como un servicio y estás usando Microsoft Edge como explorador, conecta la puerta de enlace a Azure puede presentar errores después generando una nueva ventana del explorador. Intenta evitar este problema agregando https://login.microsoftonline.com, https://login.live.com, y la dirección URL de la puerta de enlace como sitios de confianza y sitios permitidos para la configuración del bloqueador de elementos emergentes en el explorador del lado cliente. Para obtener más información sobre cómo corregir esto en la [Guía de solución de problemas](troubleshooting.md#azlogin). [17990376]

- Si tienes instalado en modo de escritorio de Windows Admin Center, la pestaña del explorador en Microsoft Edge no mostrar la dirección. [17665801]

### Google Chrome

- Antes de la versión 70 (liberado finales de octubre de 2018) Chrome tenía un [error](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) en relación con el protocolo websockets y la autenticación NTLM. Esto afecta a las siguientes herramientas: Eventos, PowerShell, Escritorio remoto.

- Chrome puede mostrar varias peticiones de credenciales, especialmente durante la adición de la experiencia de conexión en un entorno de **grupo de trabajo** (que no sea dominio).

- Si tienes Windows Admin Center implementado como un servicio, elementos emergentes desde la dirección URL de la puerta de enlace que esté habilitado para que ninguna funcionalidad de integración de Azure para que funcione. Estos servicios incluyen el adaptador de red de Azure, administración de actualizaciones de Azure y Azure Site Recovery.

### Mozilla Firefox

Windows Admin Center no se ha probado con Mozilla Firefox, pero deberían funcionar la mayoría de las funciones.

- Instalación de Windows 10: Mozilla Firefox tiene su propio almacén de certificados, por lo que debes importar el ```Windows Admin Center Client``` certificado en Firefox para usar Windows Admin Center en Windows 10.

<a id="websockets"></a>

## Compatibilidad de WebSocket al utilizar un servicio de proxy

Los módulos Escritorio remoto, PowerShell y Eventos en Windows Admin Center utilizan el protocolo WebSocket, que a menudo no se admite al utilizar un servicio de proxy. El soporte de Websocket en Proxy de aplicación de AzureAD se encuentra en la [vista previa](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) y buscando comentarios sobre compatibilidad.

## Soporte para las versiones de Windows Server antes de 2016 (R2 2012, 2012, 2008 R2)

> [!NOTE]
> Windows Admin Center requiere características de PowerShell que no se incluyen en Windows Server 2012 R2, 2012 o 2008 R2. Si vas a administrar Windows Server con Windows Admin Center, debes instalar WMF versión 5.1 o posterior en esos servidores.

Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior.

Si no está instalado, puedes [descargar e instalar WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## Control de acceso basado en roles (RBAC)

- La implementación de RBAC no se realizará correctamente en equipos configurados para usar Control de aplicaciones de Windows Defender Control de aplicación de Windows Defender (WDAC, anteriormente conocido como Integridad de código). [16568455]

- Para usar RBAC en un clúster, debes implementar individualmente la configuración en cada nodo miembro.

- Cuando se implementa RBAC, es posible que obtengas errores no autorizados que se atribuyen incorrectamente a la configuración de RBAC. [16369238]

## Solución del Administrador de servidores

### Configuración del servidor

- Si puedes modificar una opción de configuración y luego intentan Desplázate sin guardar, la página avisa sobre los cambios, pero seguir Desplázate. Puedes terminar en un estado donde la pestaña de configuración que se ha seleccionado no coincide con el contenido de la página. [19905798] [19905787]

### Certificados

- No se puede importar un certificado cifrado .PFX en al almacén del usuario actual. [11818622]

### Dispositivos

- Cuando se navega a través de la tabla con el teclado, la selección puede saltar a la parte superior del grupo de la tabla. [16646059]

### Eventos

- Los eventos se realiza a través de [compatibilidad de websocket al usar un servicio de proxy.](#websockets)

- Puedes obtener un error que hace referencia al "tamaño del paquete" al exportar los archivos de registro de gran tamaño. [16630279]

  - Para resolver este problema, usa el siguiente comando en un símbolo del sistema con privilegios elevados en el equipo de la puerta de enlace: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### Archivos

- Aún no se admite la carga o descarga de archivos grandes. (límite \~100 mb) [12524234]

### PowerShell

- PowerShell se realiza a través de [compatibilidad de websocket cuando se usa un servicio de proxy](#websockets)

- Pegar con un solo clic con el botón derecho en la consola de PowerShell del escritorio no funciona. En su lugar, obtendrás el menú contextual del explorador, donde puedes seleccionar pegar. Ctrl-V funciona bien.

- Ctrl-C para copiar no funciona, siempre enviará el comando de salto de Ctrl-C a la consola. Copia con el menú contextual al hacer clic con el botón derecho funciona.

- Cuando reduces el tamaño de la ventana de Windows Admin Center, se distribuye el contenido del terminal, pero cuando se agranda de nuevo, el contenido puede que no vuelva a su estado anterior. Si se embrollan las cosas, puedes intentar Clear-Host o desconecta y volver a conectar con el botón encima del terminal.

### Editor del registro

- No implementada la funcionalidad de búsqueda. [13820009]

### Escritorio remoto

- Es posible que producirá un error en la herramienta escritorio remoto para conectarse al administrar Windows Server 2012. [20258278]

- Al usar el escritorio remoto para conectarse a un equipo que no esté unido a un dominio, debes escribir tu cuenta en el ```MACHINENAME\USERNAME``` formato.

- Algunas configuraciones pueden bloquear el cliente de escritorio remoto del centro de administración de Windows con directivas de grupo. Si encuentras esto, habilitar ```Allow users to connect remotely by using Remote Desktop Services``` bajo ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Escritorio remoto se realiza a través [compatibilidad de websocket.](#websockets)

- La herramienta Escritorio remoto no admite actualmente copiar/pegar texto, imagen o archivo entre el escritorio local y la sesión remota.

- Para realizar una operación de copiar/pegar dentro de la sesión remota, puedes copiar como normal (clic derecho + copia o Ctrl + C), pero pegar requiere clic derecho + pegar (Ctrl+V no funciona)

- No se pueden enviar los siguientes comandos de clave a la sesión remota
  - Alt+Tab
  - Teclas de función
  - Tecla de Windows
  - Impr Pant

- La aplicación remota: después de habilitar la herramienta de la aplicación remota de la configuración de escritorio remoto, la herramienta podrían no aparecer en la lista de herramientas al administrar un servidor con experiencia de escritorio. [18906904]

### Roles y características

- Al seleccionar los roles o características con orígenes no disponibles para la instalación, se omiten. [12946914]

- Si decides no reiniciar automáticamente después de la instalación de roles, no preguntaremos de nuevo. [13098852]

- Si decides reiniciar automáticamente, se producirá el reinicio antes de que se actualice el estado al 100%. [13098852]

### Almacenamiento

- La obtención de información de cuota puede presentar errores sin una notificación de error (seguirá habiendo un error en la consola del explorador) [18962274]

- Nivel inferior: Las unidades de DVD, CD o disquete no aparecen como volúmenes en el nivel inferior.

- Nivel inferior: algunas propiedades en los volúmenes y discos no están disponible en el nivel inferior, con lo cual aparecen como desconocidos o en blanco en el panel de detalles.

- Nivel inferior: al crear un nuevo volumen, ReFS solo admite un tamaño de unidad de asignación de 64 K en equipos con Windows 2012 y 2012 R2. Si se crea un volumen ReFS con un tamaño de unidad de asignación más pequeño en destinos de nivel inferior, se producirá un error al formatear el sistema de archivos. El nuevo volumen no se podrá utilizar. La solución consiste en eliminar el volumen y usar el tamaño de unidad de asignación de 64 K.

### Actualizaciones

- Después de instalar las actualizaciones, estado de la instalación puede almacenar en caché y requiere una actualización del explorador.

- Se puede producir el error: "No existe el conjunto de claves" al intentar configurar la administración de actualizaciones de Azure. En este caso, intente los siguientes pasos de corrección en el nodo administrado:
    1. Detener el servicio de 'Servicios criptográficos'.
    2. Cambiar opciones de carpeta para mostrar archivos ocultos (si es necesario).
    3. Llegado a la carpeta de "% allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" y eliminar todo su contenido.
    4. Reinicie el servicio de 'Servicios criptográficos'.
    5. Repite la configuración de administración de actualizaciones con Windows Admin Center

### Máquinas virtuales

- Al administrar las máquinas virtuales en un host de Windows Server 2012, conecta la VM en el explorador se producirá un error de herramienta para conectarse a la máquina virtual. Descargar el archivo RDP para conectarse a la máquina virtual debería seguir funcionando. [20258278]

- Azure Site Recovery: si ASR está configurada en el host fuera WAC, podrá proteger una máquina virtual desde dentro de WAC [18972276]

- Características avanzadas disponibles en el Administrador de Hyper-V, como Administrador de SAN virtual, Mover VM, Exportar VM, Replicación de VM actualmente no son compatibles.

### Conmutadores virtuales

- Switch Embedded Teaming (SET): al agregar NIC a un equipo, deben estar en la misma subred.

## Solución de administración de equipos

La solución de administración de equipos contiene un subconjunto de herramientas de la solución del Administrador de servidores, por lo que se aplican los mismos problemas conocidos, así como los siguientes problemas específicos de la solución de administración de equipos:

- Si usas una Account de Microsoft ([MSA](https://account.microsoft.com/account/)) o si usas Azure Active Directory (AAD) para iniciar sesión en la máquina de Windows 10, debes especificar "administrar-como" credenciales para administrar el equipo local [16568455]

- Cuando intentas administrar el localhost, se te pedirá elevar el proceso de puerta de enlace. Si haces clic en **no** en la ventana emergente de Control de cuentas de usuario que aparece, Windows Admin Center no podrá volver a mostrarlo. En este caso, sal del proceso de puerta de enlace haciendo doble clic con el botón derecho en el icono de Windows Admin Center en la bandeja del sistema y elige Salir y vuelve a iniciar Windows Admin Center desde el menú Inicio.

- Windows 10 no dispone de comunicación remota de PowerShell/WinRM de forma predeterminada
  
  - Para habilitar la administración del cliente de Windows 10, debes ejecutar el comando ```Enable-PSRemoting``` desde un símbolo del sistema con privilegios elevados de PowerShell.

  - También es posible que tengas que actualizar tu firewall para permitir conexiones desde fuera de la subred local con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Para escenarios de redes más restrictivos, consulta [esta documentación](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## Solución Administrador de clústeres de conmutación por error

- Al administrar un clúster, (ya sea Hiperconvergido o traditional) podrías encontrar un error del tipo **no se encontró el shell**. Si esto ocurre, vuelve a cargar el explorador o desplázate a otra herramienta y vuelve. [13882442]

- Puede producirse un problema al administrar un clúster de nivel inferior (Windows Server 2012 o 2012 R2) que no se haya configurado completamente. La solución para este problema es asegurarse de que la característica de Windows **RSAT-Clustering-PowerShell** se ha instalado y habilitado en **cada nodo miembro** del clúster. Para hacer esto con PowerShell, escribe el comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` en todos los nodos del clúster. [12524664]

- Es posible que se deba agregar el clúster con el FQDN completo para que se detecte correctamente.

- Al conectar a un clúster con Windows Admin Center instalado como una puerta de enlace y proporcionando el nombre de usuario/contraseña explícitos para autenticar, debes seleccionar **Usar estas credenciales para todas las conexiones** para que las credenciales estén disponibles para consultar los nodos miembros.

## Solución Administrador de clústeres hiperconvergidos

- Algunos comandos, como **Unidades - Actualizar firmware**, **Servidores - Eliminar** y **Volúmenes - Abrir** se han deshabilitado y actualmente no se admiten.