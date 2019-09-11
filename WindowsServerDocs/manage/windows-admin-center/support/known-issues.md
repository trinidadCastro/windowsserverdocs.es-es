---
title: Problemas conocidos de Windows Admin Center
description: Problemas conocidos de Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: b222cd4b97beecd25c14b9f8f39627bf46cb7716
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869539"
---
# <a name="windows-admin-center-known-issues"></a>Problemas conocidos de Windows Admin Center

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Si encuentras un problema no descrito en esta página, [háznoslo saber](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Integrador de Lenovo XClarity

El problema de incompatibilidad previamente divulgado de la extensión de integrador de Lenovo XClarity y el centro de administración de Windows versión 1904 ahora se resuelve con la versión 1904,1 del centro de administración de Windows. Le recomendamos encarecidamente que actualice a la última versión compatible del centro de administración de Windows.

- La versión 1,1 de la extensión de integrador de Lenovo XClarity es totalmente compatible con el centro de administración de Windows 1904,1. Le recomendamos encarecidamente que actualice a la versión más reciente del centro de administración de Windows y de la extensión de Lenovo.
- Por cualquier motivo, si necesita seguir usando el centro de administración de Windows 1809,5 por el momento, puede usar XClarity Integrator 1.0.4, que también estará disponible en la fuente de extensión del centro de administración de Windows hasta que el centro de administración de Windows 1809,5 ya no se admita en función de nuestra [Directiva de soporte técnico](../support/index.md).

## <a name="installer"></a>Instalador

- Al instalar Windows Admin Center con su propio certificado, ten en cuenta que si copias la huella digital desde la herramienta MMC del Administrador de certificados, [contendrá un carácter no válido al principio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como una solución alternativa, escribe el primer carácter de la huella digital y copiar/pega el resto.

- No se admite el uso de un puerto inferior a 1024. En el modo de servicio, puede configurar opcionalmente el puerto 80 para redirigir al puerto especificado.

- Si el servicio Windows Update (wuauserv) está detenido y deshabilitado, se producirá un error en el instalador. [19100629]

### <a name="upgrade"></a>Actualizar

- Al actualizar el centro de administración de Windows en modo de servicio desde una versión anterior, si usa msiexec en modo silencioso, es posible que se produzca un problema en el que se elimine la regla de Firewall de entrada para el puerto del centro de administración de Windows.
  - Para volver a crear la regla, ejecute el siguiente comando desde una consola de PowerShell con \<privilegios elevados, reemplazando el puerto > por el puerto configurado para el centro de administración de Windows (el valor predeterminado es 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>General

- Si tiene el centro de administración de Windows instalado como puerta de enlace en **Windows Server 2016** bajo un uso intensivo, el servicio puede bloquearse con un error en ```Faulting application name: sme.exe``` el ```Faulting module name: WsmSvc.dll```registro de eventos que contiene y. Esto se debe a un error que se ha corregido en Windows Server 2019. La revisión para Windows Server 2016 se incluyó la actualización acumulativa de febrero de 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si el centro de administración de Windows está instalado como una puerta de enlace y la lista de conexiones parece estar dañada, siga estos pasos:

   > [!WARNING]
   >Esto eliminará la lista de conexiones y la configuración de todos los usuarios del centro de administración de Windows en la puerta de enlace.

  1. Desinstala Windows Admin Center
  2. Elimina la carpeta **Server Management Experience** en **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Volver a instalar Windows Admin Center

- Si deja abierta la herramienta y está inactiva durante un largo período de tiempo, puede obtener varios **errores: El estado de espacio de ejecución no es válido** para estos errores de operación. Si esto ocurre, actualiza el explorador. Si encuentra esto, [envíenos sus comentarios](http://aka.ms/WACfeedback).

- Es posible que encuentres un **Error 500** al actualizar páginas con direcciones URL muy largas. [12443710]

- En algunas herramientas, el corrector ortográfico del explorador puede marcar ciertos valores de campo como errores ortográficos. [12425477]

- En algunas herramientas, los botones de comando no pueden reflejar los cambios de estado inmediatamente después de hacer clic y la herramienta de la interfaz de usuario puede que no refleje automáticamente los cambios realizados en determinadas propiedades. Puedes hacer clic en **Actualizar** para recuperar el estado más reciente desde el servidor de destino. [11445790]

- Filtrado de etiquetas en la lista de conexiones: Si selecciona conexiones con las casillas de selección múltiple, filtre la lista de conexiones por etiquetas, la selección original se mantiene para que cualquier acción que seleccione se aplique a todas las máquinas seleccionadas anteriormente. [18099259]

- Puede haber una desviación mínima entre el número de versión de los sistemas operativos que se ejecutan en los módulos del centro de administración de Windows y lo que se incluye en el aviso de software de terceros.

### <a name="extension-manager"></a>Administrador de extensiones

- Al actualizar el centro de administración de Windows, debe volver a instalar las extensiones.
- Si agrega una fuente de extensión a la que no se puede acceder, no hay ninguna advertencia. [14412861]

## <a name="browser-specific-issues"></a>Problemas específicos del explorador

### <a name="microsoft-edge"></a>Microsoft Edge

- En algunos casos, puedes encontrar tiempos de carga largos al utilizar Microsoft Edge para acceder a una puerta de enlace de Windows Admin Center a través de Internet. Esto puede ocurrir en VM de Azure donde la puerta de enlace de Windows Admin Center está usando un certificado autofirmado. [13819912]

- Al utilizar Azure Active Directory como proveedor de identidades y Windows Admin Center se configura con un certificado autofirmado o, en caso contrario , no de confianza, no puedes completar la autenticación de AAD en Microsoft Edge.  [15968377]

- Si tiene el centro de administración de Windows implementado como un servicio y usa Microsoft Edge como su explorador, es posible que se produzca un error al conectar la puerta de enlace a Azure después de generar una nueva ventana del explorador. Intente solucionar este problema agregando https://login.microsoftonline.com , https://login.live.com y la dirección URL de la puerta de enlace como sitios de confianza y sitios permitidos para la configuración del bloqueador de elementos emergentes en el explorador del lado cliente. Para obtener más información sobre cómo solucionar este [problema](troubleshooting.md#azure-features-dont-work-properly-in-edge)en la guía de solución de problemas. [17990376]

- Si el centro de administración de Windows está instalado en el modo de escritorio, la pestaña explorador de Microsoft Edge no mostrará favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Antes de la versión 70 (publicada a finales de octubre de 2018), Chrome tenía un [error](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) relacionado con el protocolo WebSockets y la autenticación NTLM. Esto afecta a las siguientes herramientas: Eventos, PowerShell Escritorio remoto.

- Chrome puede mostrar varias peticiones de credenciales, especialmente durante la adición de la experiencia de conexión en un entorno de **grupo de trabajo** (que no sea dominio).

- Si el centro de administración de Windows se implementa como un servicio, es necesario habilitar los elementos emergentes de la dirección URL de la puerta de enlace para que funcione la funcionalidad de integración de Azure. Estos servicios incluyen adaptador de red de Azure, Azure Update Management y Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center no se ha probado con Mozilla Firefox, pero deberían funcionar la mayoría de las funciones.

- Instalación de Windows 10: Mozilla Firefox tiene su propio almacén de certificados, por lo que debe importar ```Windows Admin Center Client``` el certificado en Firefox para usar el centro de administración de Windows en Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilidad de WebSocket al usar un servicio de proxy

Los módulos Escritorio remoto, PowerShell y Eventos en Windows Admin Center utilizan el protocolo WebSocket, que a menudo no se admite al utilizar un servicio de proxy. El soporte de Websocket en Proxy de aplicación de Azure AD se encuentra en la [vista previa](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) y buscando comentarios sobre compatibilidad.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Compatibilidad con versiones de Windows Server anteriores a 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> El centro de administración de Windows requiere características de PowerShell que no están incluidas en Windows Server 2012 R2, 2012 o 2008 R2. Si va a administrar Windows Server con el centro de administración de Windows, tendrá que instalar la versión 5,1 o superior de WMF en esos servidores.

Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior.

Si no está instalado, puedes [descargar e instalar WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Access Control basado en roles (RBAC)

- La implementación de RBAC no se realizará correctamente en equipos configurados para usar Control de aplicaciones de Windows Defender Control de aplicación de Windows Defender (WDAC, anteriormente conocido como Integridad de código). [16568455]

- Para usar RBAC en un clúster, debes implementar individualmente la configuración en cada nodo miembro.

- Cuando se implementa RBAC, es posible que obtengas errores no autorizados que se atribuyen incorrectamente a la configuración de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solución del Administrador de servidores

### <a name="server-settings"></a>Configuración del servidor

- Si modifica una configuración y, a continuación, intenta salir sin guardar, la página le avisará de los cambios no guardados, pero seguirá desplazarse. Puede acabar en un estado en el que la pestaña configuración seleccionada no coincida con el contenido de la página. [19905798] [19905787]

### <a name="certificates"></a>Certificados

- No se puede importar un certificado cifrado .PFX en al almacén del usuario actual. [11818622]

### <a name="devices"></a>Dispositivos

- Al desplazarse por la tabla con el teclado, la selección puede saltar a la parte superior del grupo de tablas. [16646059]

### <a name="events"></a>Events

- Los eventos se realiza a través de [compatibilidad de websocket al usar un servicio de proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Puedes obtener un error que hace referencia al "tamaño del paquete" al exportar los archivos de registro de gran tamaño. [16630279]

  - Para resolver este error, use el siguiente comando en un símbolo del sistema con privilegios elevados en el equipo de puerta de enlace:```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Archivos

- Aún no se admite la carga o descarga de archivos grandes. (\~límite de 100 MB) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell se realiza a través de [compatibilidad de websocket cuando se usa un servicio de proxy](#websocket-compatibility-when-using-a-proxy-service)

- Pegar con un solo clic con el botón derecho en la consola de PowerShell del escritorio no funciona. En su lugar, obtendrás el menú contextual del explorador, donde puedes seleccionar pegar. Ctrl-V funciona bien.

- Ctrl-C para copiar no funciona, siempre enviará el comando de salto de Ctrl-C a la consola. Copia con el menú contextual al hacer clic con el botón derecho funciona.

- Cuando reduces el tamaño de la ventana de Windows Admin Center, se distribuye el contenido del terminal, pero cuando se agranda de nuevo, el contenido puede que no vuelva a su estado anterior. Si se embrollan las cosas, puedes intentar Clear-Host o desconecta y volver a conectar con el botón encima del terminal.

### <a name="registry-editor"></a>Editor del Registro

- No implementada la funcionalidad de búsqueda. [13820009]

### <a name="remote-desktop"></a>Escritorio remoto

- La herramienta de Escritorio remoto puede no conectarse al administrar Windows Server 2012. [20258278]

- Al usar el escritorio remoto para conectarse a un equipo que no está unido a un dominio, debe especificar su cuenta en ```MACHINENAME\USERNAME``` el formato.

- Algunas configuraciones pueden bloquear el cliente de escritorio remoto del centro de administración de Windows con la Directiva de grupo. Si encuentra esto, habilítelo ```Allow users to connect remotely by using Remote Desktop Services``` en```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Escritorio remoto se ve afectado por la [compatibilidad de WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- La herramienta Escritorio remoto no admite actualmente copiar/pegar texto, imagen o archivo entre el escritorio local y la sesión remota.

- Para realizar una operación de copiar/pegar dentro de la sesión remota, puedes copiar como normal (clic derecho + copia o Ctrl + C), pero pegar requiere clic derecho + pegar (Ctrl+V no funciona)

- No se pueden enviar los siguientes comandos de clave a la sesión remota
  - Alt+Tab
  - Teclas de función
  - Tecla de Windows
  - Impr Pant

- Aplicación remota: después de habilitar la herramienta de aplicación remota desde Escritorio remoto configuración, es posible que la herramienta no aparezca en la lista de herramientas al administrar un servidor con experiencia de escritorio. [18906904]

### <a name="roles-and-features"></a>Roles y características

- Al seleccionar los roles o características con orígenes no disponibles para la instalación, se omiten. [12946914]

- Si decides no reiniciar automáticamente después de la instalación de roles, no preguntaremos de nuevo. [13098852]

- Si decides reiniciar automáticamente, se producirá el reinicio antes de que se actualice el estado al 100 %. [13098852]

### <a name="storage"></a>Almacenamiento

- Se puede producir un error en la captura de información de cuota sin una notificación de error (todavía habrá un error en la consola del explorador) [18962274]

- Nivel inferior: Las unidades de DVD/CD/disquete no aparecen como volúmenes en el nivel inferior.

- Nivel inferior: Algunas propiedades de volúmenes y discos no están disponibles de nivel inferior, por lo que parecen desconocidos o en blanco en el panel de detalles.

- Nivel inferior: Al crear un nuevo volumen, ReFS solo admite un tamaño de unidad de asignación de 64 k en máquinas con Windows 2012 y 2012 R2. Si se crea un volumen ReFS con un tamaño de unidad de asignación más pequeño en destinos de nivel inferior, se producirá un error al formatear el sistema de archivos. El nuevo volumen no se podrá utilizar. La solución consiste en eliminar el volumen y usar el tamaño de unidad de asignación de 64 K.

### <a name="updates"></a>Actualizaciones

- Después de instalar las actualizaciones, el estado de la instalación puede almacenarse en caché y requerir una actualización del explorador.

- Es posible que se produzca el error: "El conjunto de claves no existe" al intentar configurar la administración de actualizaciones de Azure. En este caso, pruebe los siguientes pasos de corrección en el nodo administrado:
    1. Detenga el servicio "servicios criptográficos".
    2. Cambie las opciones de carpeta para mostrar los archivos ocultos (si es necesario).
    3. Vaya a la carpeta "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" y elimine todo su contenido.
    4. Reinicie el servicio "servicios criptográficos".
    5. Repetir la configuración de Update Management con el centro de administración de Windows

### <a name="virtual-machines"></a>Virtual Machines

- Al administrar las máquinas virtuales en un host de Windows Server 2012, la herramienta de conexión de máquina virtual en el explorador no podrá conectarse a la máquina virtual. La descarga del archivo. RDP para conectarse a la máquina virtual debería seguir funcionando. [20258278]

- Azure Site Recovery: si ASR está configurado en el host fuera de WAC, no podrá proteger una máquina virtual desde WAC [18972276]

- Características avanzadas disponibles en el Administrador de Hyper-V, como Administrador de SAN virtual, Mover VM, Exportar VM, Replicación de VM actualmente no son compatibles.

### <a name="virtual-switches"></a>Conmutadores virtuales

- Cambiar la formación de equipos incrustada (SET): Al agregar NIC a un equipo, deben estar en la misma subred.

## <a name="computer-management-solution"></a>Solución de administración de equipos

La solución de administración de equipos contiene un subconjunto de herramientas de la solución del Administrador de servidores, por lo que se aplican los mismos problemas conocidos, así como los siguientes problemas específicos de la solución de administración de equipos:

- Si usa una cuenta Microsoft ([MSA](https://account.microsoft.com/account/)) o si usa Azure Active Directory (AAD) para iniciar sesión en su equipo con Windows 10, debe especificar las credenciales "administrar como" para administrar el equipo local [16568455]

- Cuando intentas administrar el localhost, se te pedirá elevar el proceso de puerta de enlace. Si haces clic en **no** en la ventana emergente de Control de cuentas de usuario que aparece, Windows Admin Center no podrá volver a mostrarlo. En este caso, sal del proceso de puerta de enlace haciendo doble clic con el botón derecho en el icono de Windows Admin Center en la bandeja del sistema y elige Salir y vuelve a iniciar Windows Admin Center desde el menú Inicio.

- Windows 10 no dispone de comunicación remota de PowerShell/WinRM de forma predeterminada
  
  - Para habilitar la administración del cliente de Windows 10, debes ejecutar el comando ```Enable-PSRemoting``` desde un símbolo del sistema con privilegios elevados de PowerShell.

  - También es posible que tengas que actualizar tu firewall para permitir conexiones desde fuera de la subred local con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Para escenarios de redes más restrictivos, consulta [esta documentación](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Solución Administrador de clústeres de conmutación por error

- Al administrar un clúster, (ya sea Hiperconvergido o traditional) podrías encontrar un error del tipo **no se encontró el shell**. Si esto ocurre, vuelve a cargar el explorador o desplázate a otra herramienta y vuelve. [13882442]

- Puede producirse un problema al administrar un clúster de nivel inferior (Windows Server 2012 o 2012 R2) que no se haya configurado completamente. La solución para este problema es asegurarse de que la característica de Windows **RSAT-Clustering-PowerShell** se ha instalado y habilitado en **cada nodo miembro** del clúster. Para hacer esto con PowerShell, escribe el comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` en todos los nodos del clúster. [12524664]

- Es posible que se deba agregar el clúster con el FQDN completo para que se detecte correctamente.

- Al conectar a un clúster con Windows Admin Center instalado como una puerta de enlace y proporcionando el nombre de usuario/contraseña explícitos para autenticar, debes seleccionar **Usar estas credenciales para todas las conexiones** para que las credenciales estén disponibles para consultar los nodos miembros.

## <a name="hyper-converged-cluster-manager-solution"></a>Solución Administrador de clústeres hiperconvergidos

- Algunos comandos, como **Unidades - Actualizar firmware**, **Servidores - Eliminar** y **Volúmenes - Abrir** se han deshabilitado y actualmente no se admiten.