---
title: Solución avanzada de problemas de bloque de mensajes del servidor (SMB)
description: Presenta los métodos avanzados para solucionar problemas de bloque de mensajes del servidor (SMB).
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 88f707cc02514dc13c212c6462c47e4d3a94b5f5
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654586"
---
# <a name="advanced-troubleshooting-server-message-block-smb"></a>Solución avanzada de problemas de bloque de mensajes del servidor (SMB)

El bloque de mensajes del servidor (SMB) es un protocolo de transporte de red para las operaciones del sistema de archivos para permitir que un cliente tenga acceso a los recursos de un servidor. El propósito principal del protocolo SMB es habilitar el acceso al sistema de archivos remoto entre dos sistemas a través de TCP/IP.

La solución de problemas de SMB puede ser muy compleja. En su lugar, este artículo no es una guía exhaustiva de solución de problemas. es una breve descripción de los aspectos básicos de cómo solucionar problemas de forma eficaz de SMB.

## <a name="tools-and-data-collection"></a>Herramientas y recopilación de datos

Uno de los aspectos clave de la solución de problemas de SMB de calidad es comunicar la terminología correcta. Por lo tanto, en este artículo se presenta la terminología básica de SMB para garantizar la precisión de la recopilación y el análisis de datos.

> [!Note]
> El *servidor SMB (SRV)* hace referencia al sistema que hospeda el sistema de archivos, también conocido como servidor de archivos. *El cliente SMB (CLI)* hace referencia al sistema que está intentando tener acceso al sistema de archivos, independientemente de la versión o edición del sistema operativo.

Por ejemplo, si usa Windows Server 2016 para llegar a un recurso compartido de SMB que se hospeda en Windows 10, Windows Server 2016 es el cliente SMB y el servidor SMB de Windows 10.

### <a name="collect-data"></a>Recopilar datos

Antes de solucionar problemas de SMB, se recomienda que primero recopile un seguimiento de red tanto en el lado cliente como en el servidor. Se aplican las directrices siguientes:

- En los sistemas Windows, puede usar NetShell (netsh), Monitor de red, el analizador de mensajes o Wireshark para recopilar un seguimiento de red.

- Los dispositivos de terceros generalmente tienen una herramienta de captura de paquetes integrada, como tcpdump (Linux/FreeBSD/UNIX) o pktt (NetApp). Por ejemplo, si el cliente SMB o el servidor SMB es un host de UNIX, puede recopilar datos ejecutando el comando siguiente:
  
  ```cmd
  # tcpdump -s0 -n -i any -w /tmp/$(hostname)-smbtrace.pcap
  ```
  
  Detenga la recopilación de datos con **Ctrl + C** desde el teclado.

Para detectar el origen del problema, puede comprobar los seguimientos de dos lados: CLI, SRV o en algún punto entre.

#### <a name="using-netshell-to-collect-data"></a>Usar NetShell para recopilar datos

En esta sección se proporcionan los pasos para usar NetShell para recopilar el seguimiento de red.

> [!NOTE]  
> Un seguimiento de Netsh crea un archivo ETL. Los archivos ETL solo se pueden abrir en el analizador de mensajes (MA) y Monitor de red 3,4 (establezca el analizador en Monitor de red analizadores \> Windows).

1. En el servidor SMB y el cliente SMB, cree una carpeta **temporal** en la unidad **C**. A continuación, ejecute el siguiente comando:

   ```cmd
   netsh trace start capture=yes report=yes scenario=NetConnection level=5 maxsize=1024 tracefile=c:\\Temp\\%computername%\_nettrace.etl**
   ```
   
   Si usa PowerShell, ejecute los siguientes cmdlets:
   
   ```PowerShell
   New-NetEventSession -Name trace -LocalFilePath "C:\Temp\$env:computername`_netCap.etl" -MaxFileSize 1024

   Add-NetEventPacketCaptureProvider -SessionName trace -TruncationLength 1500

   Start-NetEventSession trace
   ```
   
2. Reproduzca el problema.

3. Ejecute el comando siguiente para detener el seguimiento:

   ```cmd
   netsh trace stop
   ```
   
   Si usa PowerShell, ejecute los siguientes cmdlets:

   ```PowerShell
   Stop-NetEventSession trace  
   Remove-NetEventSession trace
   ```

> [!NOTE] 
> Solo debe realizar un seguimiento de la cantidad mínima de los datos que se transfieren. En el caso de los problemas de rendimiento, tome siempre un seguimiento bueno y malo, si la situación lo permite.

### <a name="analyze-the-traffic"></a>Analizar el tráfico

SMB es un protocolo de nivel de aplicación que usa TCP/IP como protocolo de transporte de red. Por lo tanto, un problema de SMB también puede deberse a problemas de TCP/IP.

Compruebe si TCP/IP experimenta cualquiera de estos problemas:

1. El protocolo de enlace de tres vías TCP no finaliza. Esto suele indicar que hay un bloque de firewall o que el servicio de servidor no se está ejecutando.

2. Se están produciendo retransmisiones. Esto puede producir transferencias lentas de archivos debido a la limitación de congestión de TCP compuesta.

3. Cinco retransmisións seguidas por un restablecimiento de TCP podrían significar que se perdió la conexión entre los sistemas, o que uno de los servicios SMB se bloqueó o dejó de responder.

4. La ventana de recepción TCP está disminuyendo. Esto puede deberse a un almacenamiento lento o a algún otro problema que impida que los datos se recuperen del búfer Winsock del controlador de función auxiliar (AFD).

Si no hay ningún problema de TCP/IP apreciable, busque errores de SMB. Para ello, sigue estos pasos:

1. Compruebe siempre los errores SMB en la especificación del protocolo MS-SMB2. Muchos errores de SMB son benignos (no dañinos). Consulte la siguiente información para determinar por qué SMB devolvió el error antes de concluir que el error está relacionado con cualquiera de los siguientes problemas:

   - En el tema de la [Sintaxis de mensajes MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/6eaf6e75-9c23-4eda-be99-c9223c60b181) se detallan los comandos SMB y sus opciones.
    
   - En el tema de [procesamiento de clientes de MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/df0625a5-6516-4fbe-bf97-01bef451cab2) se detalla cómo el cliente SMB crea solicitudes y responde a los mensajes del servidor.

   - En el tema de [procesamiento de servidor MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/e1d08834-42e0-41ca-a833-fc26f5132a6f) se detalla cómo el servidor SMB crea solicitudes y responde a las solicitudes de los clientes.

2. Compruebe si se envía un comando TCP RESET inmediatamente después de un FSCTL\_validar\_comando NEGOTIATE\_INFO (Validate Negotiate). Si es así, consulte la siguiente información:

   - La sesión de SMB debe terminar (restablecimiento de TCP) cuando se produce un error en el proceso de validación de negociación en el cliente o el servidor.

   - Este proceso puede producir un error porque un optimizador de WAN está modificando el paquete de negociación de SMB.

   - Si la conexión finalizó prematuramente, identifique la última comunicación de Exchange entre el cliente y el servidor.

#### <a name="analyze-the-protocol"></a>Analizar el protocolo

Examine los detalles reales del protocolo SMB en el seguimiento de red para comprender los comandos y las opciones exactos que se usan.

> [!NOTE]
> Solo el analizador de mensajes puede analizar SMBv3 y los comandos de la versión posterior.

- Recuerde que SMB solo hace lo que se le dice hacer.

- Puede obtener información sobre lo que la aplicación está tratando de hacer examinando los comandos SMB.

Compare los comandos y las operaciones con la especificación del protocolo para asegurarse de que todo funciona correctamente. Si no es así, recopile los datos que estén más cerca o en un nivel inferior para buscar más información sobre la causa raíz. Para ello, sigue estos pasos:

1. Recopilar una captura de paquetes estándar.

2. Ejecute el comando **netsh** para realizar un seguimiento de los detalles y recopilarlos sobre si hay problemas en la pila de red o en las aplicaciones de la plataforma de filtrado de Windows (WFP), como firewall o programa antivirus.

3. Si se produce un error en todas las demás opciones, recopile un t. cmd si sospecha que el problema se produce en el propio SMB o si ninguno de los otros datos es suficiente para identificar una causa raíz.

Por ejemplo:

- Experimenta transferencias de archivos lentas a un único servidor de archivos.

- Los seguimientos de dos lados muestran que el SRV responde lentamente a una solicitud de lectura.

- Al quitar un programa antivirus, se resuelven las transferencias de archivos lentas.

- Póngase en contacto con el programa antivirus manufactory para resolver el problema.

> [!NOTE]
> Opcionalmente, **también** puede desinstalar temporalmente el programa antivirus durante la solución de problemas.

#### <a name="event-logs"></a>Registros de eventos

Tanto el cliente SMB como el servidor SMB tienen una estructura de registro de eventos detallada, tal como se muestra en la captura de pantalla siguiente. Recopile los registros de eventos para ayudar a encontrar la causa principal del problema.

![Registros de eventos](media/troubleshooting-smb-1.png)

## <a name="smb-related-system-files"></a>Archivos del sistema relacionados con SMB

En esta sección se enumeran los archivos del sistema relacionados con SMB. Para mantener actualizados los archivos del sistema, asegúrese de que está instalado el [paquete acumulativo de actualizaciones](https://support.microsoft.com/en-us/help/4498140/windows-10-update-history) más reciente.

Los archivos binarios de cliente SMB que se enumeran en **% WINDIR%\\system32\\controladores**:

- RDBSS. sys

- MRXSMB. sys

- MRXSMB10. sys

- MRXSMB20. sys

- MUP. sys

- SMBdirect. sys

Los archivos binarios del servidor SMB que aparecen en **% WINDIR%\\system32\\controladores**:

- SRVNET. sys

- SRV. sys

- SRV2. sys

- SMBdirect. sys

- En **% WINDIR%\\system32**

- srvsvc. dll

![Componentes de SMB](media/troubleshooting-smb-2.png)

### <a name="update-suggestions"></a>Sugerencias de actualización

Se recomienda actualizar los siguientes componentes antes de solucionar problemas de SMB:

- Un servidor de archivos requiere almacenamiento de archivos. Si el almacenamiento tiene un componente iSCSI, actualice dichos componentes.

- Actualice los componentes de red.

- Para mejorar el rendimiento y la estabilidad, actualice Windows Core.

## <a name="reference"></a>Referencia

[Escenario de intercambio de paquetes del protocolo SMB de Microsoft](https://docs.microsoft.com/windows/win32/fileio/microsoft-smb-protocol-packet-exchange-scenario)