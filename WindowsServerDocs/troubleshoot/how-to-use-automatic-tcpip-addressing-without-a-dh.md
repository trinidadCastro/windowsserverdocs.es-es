---
title: Uso del direccionamiento automático de TCP/IP sin un servidor DHCP
description: Presente cómo usar el direccionamiento automático de TCP/IP sin un servidor DHCP.
ms.prod: windows-server
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: troubleshoot
author: Deland-Han
ms.author: delhan
ms.reviewer: robsmi
ms.openlocfilehash: fcd85c29975709053009ec4a2684df88b4bafd69
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409806"
---
# <a name="how-to-use-automatic-tcpip-addressing-without-a-dhcp-server"></a>Uso del direccionamiento automático de TCP/IP sin un servidor DHCP

En este artículo se describe cómo usar el direccionamiento automático del Protocolo de control de transmisión/Protocolo de Internet (TCP/IP) sin un servidor de protocolo de configuración dinámica de host (DHCP) presente en la red. Las versiones de sistema operativo enumeradas en la sección "se aplica a" de este artículo tienen una característica denominada direcciones IP privadas automáticas (APIPA). Con esta característica, un equipo Windows puede asignarse a sí mismo una dirección de protocolo de Internet (IP) en caso de que un servidor DHCP no esté disponible o no exista en la red. Esta característica hace que la configuración y compatibilidad de una pequeña red de área local (LAN) que ejecuta TCP/IP sea menos difícil.

## <a name="more-information"></a>Más información

> [!IMPORTANT]
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/help/322756), por si se produjeran problemas.

Un equipo basado en Windows que esté configurado para usar DHCP puede asignarse automáticamente a una dirección de protocolo de Internet (IP) si no hay ningún servidor DHCP disponible. Por ejemplo, esto puede ocurrir en una red que no tenga un servidor DHCP o en una red si un servidor DHCP está inactivo temporalmente para el mantenimiento.

La autoridad de asignación de números de Internet (IANA) ha reservado 169.254.0.0-169.254.255.255 para direcciones IP privadas automáticas. Como resultado, APIPA proporciona una dirección que se garantiza que no entra en conflicto con las direcciones enrutables.

Una vez que se ha asignado una dirección IP al adaptador de red, el equipo puede usar TCP/IP para comunicarse con cualquier otro equipo que esté conectado a la misma LAN y que también esté configurado para APIPA o que tenga la dirección IP establecida manualmente en 169.254. x. y (donde x. y es el intervalo de direcciones del cliente) con la máscara de subred 255.255.0.0. Tenga en cuenta que el equipo no puede comunicarse con equipos de otras subredes o con equipos que no usan direcciones IP privadas automáticas. El direccionamiento IP privado automático está habilitado de forma predeterminada.

Puede que desee deshabilitarlo en cualquiera de los siguientes casos:

- La red utiliza enrutadores.

- La red está conectada a Internet sin un servidor proxy o NAT.

A menos que haya deshabilitado los mensajes relacionados con DHCP, los mensajes DHCP le proporcionan una notificación al cambiar entre el direccionamiento DHCP y el direccionamiento IP privado automático. Si la mensajería de DHCP se deshabilita accidentalmente, puede volver a activar los mensajes DHCP cambiando el valor de PopupFlag en la siguiente clave del registro de 00 a 01:`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

Tenga en cuenta que debe reiniciar el equipo para que el cambio surta efecto. También puede determinar si el equipo usa APIPA mediante la herramienta winipcfg en Windows Millennium Edition, Windows 98 o Windows 98 Segunda edición:

Haga clic en Inicio y en ejecutar, escriba "Winipcfg" (sin las comillas) y, a continuación, haga clic en Aceptar. Haga clic en más información. Si el cuadro Dirección de configuración automática de IP contiene una dirección IP en el intervalo 169.254. x. x, el direccionamiento IP privado automático está habilitado. Si el cuadro Dirección IP existe, el direccionamiento IP privado automático no está habilitado actualmente.
En Windows 2000, Windows XP o Windows Server 2003, puede determinar si el equipo usa APIPA mediante el comando IPconfig en un símbolo del sistema:

Haga clic en Inicio y en ejecutar, escriba "cmd" (sin las comillas) y, a continuación, haga clic en Aceptar para abrir una ventana de línea de comandos de MS-DOS. Escriba "ipconfig/all" (sin las comillas) y, a continuación, presione la tecla entrar. Si la línea "configuración automática habilitada" indica "sí" y la "dirección IP de configuración automática" es 169.254. x. y (donde x. y es el identificador único del cliente), el equipo usa APIPA. Si la línea "configuración automática habilitada" indica "no", el equipo no está usando APIPA actualmente.
Puede deshabilitar la dirección IP privada automática mediante cualquiera de los métodos siguientes.

Puede configurar manualmente la información de TCP/IP, lo que deshabilita DHCP por completo. Puede deshabilitar el direccionamiento IP privado automático (pero no DHCP) editando el registro. Para ello, agregue la entrada del registro DWORD "IPAutoconfigurationEnabled" con un valor de 0X0 a la siguiente clave del registro para Windows Millennium Edition, Windows98 o Windows 98 Second Edition:`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

En Windows 2000, Windows XP y Windows Server 2003, se puede deshabilitar APIPA agregando la entrada del registro DWORD "IPAutoconfigurationEnabled" con un valor de 0X0 a la siguiente clave del registro:`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\<Adapter GUID>`
> [!NOTE]
> La subclave **GUID del adaptador** es un identificador único global (GUID) para el adaptador de LAN del equipo.

Si se especifica un valor de 1 para la entrada DWORD IPAutoconfigurationEnabled, se habilitará APIPA, que es el estado predeterminado cuando se omite este valor del registro.

## <a name="examples-of-where-apipa-may-be-useful"></a>Ejemplos de dónde puede ser útil APIPA

### <a name="example-1-no-previous-ip-address-and-no-dhcp-server"></a>Ejemplo 1: no hay ninguna dirección IP anterior y ningún servidor DHCP

Cuando el equipo basado en Windows (configurado para DHCP) se está inicializando, difunde tres o más mensajes de "detección". Si un servidor DHCP no responde después de la difusión de varios mensajes de detección, el equipo de Windows se asigna a sí mismo una dirección de la clase B (APIPA). Después, el equipo de Windows mostrará un mensaje de error al usuario del equipo (siempre que no se le haya asignado una dirección IP de un servidor DHCP en el pasado). El equipo de Windows enviará un mensaje de detección cada tres minutos en un intento de establecer comunicaciones con un servidor DHCP.

### <a name="example-2-previous-ip-address-and-no-dhcp-server"></a>Ejemplo 2: dirección IP anterior y sin servidor DHCP

El equipo comprueba el servidor DHCP y, si no se encuentra ninguno, se realiza un intento para ponerse en contacto con la puerta de enlace predeterminada. Si la puerta de enlace predeterminada responde, el equipo Windows conserva la dirección IP concedida previamente. Sin embargo, si el equipo no recibe una respuesta de la puerta de enlace predeterminada o si no se asigna ninguna, utiliza la característica de direccionamiento IP privado automático para asignarse a sí misma una dirección IP. Se presenta un mensaje de error al usuario y los mensajes de detección se transmiten cada 3 minutos. Una vez que un servidor DHCP entra en línea, se genera un mensaje que indica que las comunicaciones se han restablecido con un servidor DHCP.

### <a name="example-3-lease-expires-and-no-dhcp-server"></a>Ejemplo 3: la concesión expira y no hay ningún servidor DHCP

El equipo basado en Windows intenta volver a establecer la concesión de la dirección IP. Si el equipo Windows no encuentra un servidor DCHP, se asigna a sí mismo una dirección IP después de generar un mensaje de error. A continuación, el equipo difunde cuatro mensajes de detección y, después de cada 5 minutos, repite el procedimiento completo hasta que un servidor DHCP entra en línea. A continuación, se genera un mensaje que indica que se han restablecido las comunicaciones con el servidor DHCP.
