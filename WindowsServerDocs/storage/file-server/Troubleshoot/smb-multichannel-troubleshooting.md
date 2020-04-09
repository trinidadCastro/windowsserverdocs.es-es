---
title: Solución de problemas de SMB multicanal
description: Presenta los métodos de solución de problemas de SMB multicanal.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 210bc2057f25dc196fe9d76495c42f76c8b36311
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815348"
---
# <a name="smb-multichannel-troubleshooting"></a>Solución de problemas de SMB multicanal

En este artículo se describe cómo solucionar problemas relacionados con SMB multicanal.

## <a name="check-the-network-interface-status"></a>Comprobar el estado de la interfaz de red

Asegúrese de que el enlace para la interfaz de red está establecido en **true** en el cliente SMB (MS\_Client) y en el servidor SMB (MS\_Server). Al ejecutar el comando siguiente, la salida debería mostrar **true** en **habilitado** para ambas interfaces de red:

```PowerShell
Get-NetAdapterBinding -ComponentID ms_server,ms_msclient
```

Después, asegúrese de que la interfaz de red aparece en la salida de los siguientes comandos:

```PowerShell
Get-SmbServerNetworkInterface
```

```PowerShell
Get-SmbClientNetworkInterface
```

También puede ejecutar el comando **Get-NetAdapter** para ver el índice de la interfaz y comprobar el resultado. El índice de interfaz muestra todos los adaptadores de SMB activos que están enlazados activamente a la interfaz adecuada.

## <a name="check-the-firewall"></a>Comprobar el Firewall

Si solo hay una dirección IP local de vínculo y no hay ninguna dirección enrutable públicamente, es probable que el perfil de red esté establecido en **público**. Esto significa que SMB está bloqueado en el Firewall de forma predeterminada.

El siguiente comando revela qué Perfil de conexión se está usando. También puede usar el centro de redes y recursos compartidos para recuperar esta información.

**Get-NetConnectionProfile**

En el grupo **compartir archivos e impresoras** , compruebe las reglas de entrada del firewall para asegurarse de que "SMB de entrada" esté habilitado para el perfil correcto.

![Reglas de SMB-in](media/smb-multichannel-troubleshooting-1.png)

También puede habilitar el **uso compartido de archivos e impresoras** en la ventana **centro de redes y recursos compartidos** . Para ello, seleccione **Cambiar configuración de uso compartido avanzado** en el menú de la izquierda y, a continuación, seleccione **activar el uso compartido de archivos e impresoras** para el perfil. Esta opción habilita las reglas de Firewall compartir archivos e impresoras.

![Cambiar configuración de uso compartido avanzado](media/smb-multichannel-troubleshooting-2.png)

## <a name="capture-client-and-server-sided-traffic-for-troubleshooting"></a>Capturar el tráfico del lado cliente y del servidor para solucionar problemas

Necesita la información de seguimiento de la conexión SMB que se inicia en el protocolo de enlace de tres vías TCP. Se recomienda cerrar todas las aplicaciones (especialmente el explorador de Windows) antes de iniciar la captura. Reinicie el servicio **estación de trabajo** en el cliente SMB, inicie la captura de paquetes y, a continuación, reproduzca el problema.

Asegúrese de que la conexión de SMBv3.*x* se está negociando y de que no hay nada entre el servidor y el cliente que afecte a la negociación del dialecto. SMBv2 y versiones anteriores no son compatibles con multicanal.

Busque los paquetes de la interfaz de\_de red\_INFO. Aquí es donde el cliente SMB solicita una lista de adaptadores del servidor SMB. Si estos paquetes no se intercambian, multicanal no funciona.

El servidor responde devolviendo una lista de interfaces de red válidas. A continuación, el cliente SMB lo agrega a la lista de adaptadores disponibles para multicanal. En este punto, multicanal debe iniciar y, al menos, intentar iniciar la conexión.

Para obtener más información, consulta los artículos siguientes:

- [la aplicación 3.2.4.20.10 solicita consultas a las interfaces de red del servidor](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/147adde4-d936-4597-924a-8caa3429c6b0)

- [2.2.32.5\_respuesta de información de la interfaz de\_de red](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/fcd862d1-1b85-42df-92b1-e103199f531f)

- [3.2.5.14.11 controlar una respuesta de interfaces de red](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/5459722b-1eaa-4ead-b465-284363264cad)

En los siguientes escenarios, no se puede usar un adaptador:

- Hay un problema de enrutamiento en el cliente. Esto se debe normalmente a una tabla de enrutamiento incorrecta que fuerza el tráfico a través de la interfaz equivocada.

- Se han establecido restricciones multicanal. Para obtener más información, consulte [New-SmbMultichannelConstraint](https://docs.microsoft.com/powershell/module/smbshare/new-smbmultichannelconstraint).

- Algo bloqueó los paquetes de solicitud y respuesta de la interfaz de red.

- El cliente y el servidor no se pueden comunicar a través de la interfaz de red adicional. Por ejemplo, el protocolo de enlace de tres vías TCP produjo un error, la conexión está bloqueada por un firewall, se produjo un error en la configuración de sesión, etc.

Si el adaptador y su dirección IPv6 están en la lista enviada por el servidor, el paso siguiente consiste en ver si se intentan las comunicaciones a través de esa interfaz. Filtre el seguimiento por la dirección local del vínculo y el tráfico SMB y busque un intento de conexión. Si se trata de un seguimiento de NetConnection, también puede examinar los eventos de la plataforma de filtrado de Windows (WFP) para ver si se está bloqueando la conexión.
