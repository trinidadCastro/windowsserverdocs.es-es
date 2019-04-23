---
title: Configurar clientes RADIUS
description: En este tema se proporciona información sobre cómo configurar clientes RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 851bdb0677a17567f81a2c331baad595fe40436a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839496"
---
# <a name="configure-radius-clients"></a>Configurar clientes RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para configurar servidores de acceso de red como clientes RADIUS en NPS.

Al agregar un nuevo servidor de acceso de red \(servidor VPN, el punto de acceso inalámbrico, conmutador de autenticación o el servidor de acceso telefónico\) a la red, debe agregar el servidor como un cliente RADIUS en NPS y, a continuación, configure el cliente RADIUS para comunicarse con el NPS.

>[!IMPORTANT]
>Los equipos cliente y dispositivos, como equipos portátiles, tabletas, teléfonos y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso de red - como puntos de acceso inalámbrico, conmutadores compatibles con X de 802.1X, red de privada virtual (VPN) servidores y servidores de acceso telefónico - porque usan el protocolo RADIUS para comunicarse con servidores RADIUS, como la directiva de red Servidor \(NPS\) servidores.

Este paso también es necesario cuando NPS es miembro de un grupo de servidores remotos RADIUS que está configurado en un proxy NPS. En este caso, además de realizar los pasos de esta tarea en el proxy NPS, haga lo siguiente:

- En el proxy NPS, configure un grupo de servidores remotos RADIUS que contiene el NPS.
- En el NPS remoto, configure el proxy NPS como un cliente RADIUS.

Para llevar a cabo los procedimientos de este tema, debe tener al menos un servidor de acceso de red \(servidor VPN, el punto de acceso inalámbrico, conmutador de autenticación o el servidor de acceso telefónico\) o proxy NPS instalado físicamente en la red.

## <a name="configure-the-network-access-server"></a>Configurar el servidor de acceso de red

Use este procedimiento para configurar servidores de acceso de red para su uso con NPS. Al implementar servidores de acceso de red (NAS) como clientes RADIUS, debe configurar los clientes para comunicarse con el NPSs donde los NAS están configurados como clientes.

Este procedimiento proporciona directrices generales acerca de la configuración que debe usar para configurar sus NAS; Para obtener instrucciones específicas sobre cómo configurar el dispositivo que se va a implementar en la red, consulte la documentación del producto NAS.

### <a name="to-configure-the-network-access-server"></a>Para configurar el servidor de acceso de red

1. En el servidor NAS, en **configuración RADIUS**, seleccione **autenticación RADIUS** en el puerto de protocolo de datagramas de usuario (UDP) **1812** y **cuentas RADIUS**en el puerto UDP **1813**.
2. En **servidor Authentication** o **servidor RADIUS**, especifique la dirección IP o nombre de dominio completo (FQDN), según los requisitos del dispositivo NAS de NPS. 
3. En **secreto** o **secreto compartido**, escriba una contraseña segura. Al configurar el servidor NAS como cliente RADIUS en NPS, usará la misma contraseña, por lo que no lo olvide.
4. Si usa PEAP o EAP como método de autenticación, configure el NAS para usar la autenticación de EAP.
5. Si está configurando un punto de acceso inalámbrico en **SSID**, especifique un identificador \(SSID\), que es una cadena alfanumérica que actúa como el nombre de red. Este nombre se difunde por los puntos de acceso a los clientes inalámbricos y es visible para los usuarios con su fidelidad inalámbrica \(Wi-Fi\) puntos de conexión.
6. Si está configurando un punto de acceso inalámbrico en **802.1X y WPA**, habilitar **autenticación IEEE 802.1X** si desea implementar PEAP-MS-CHAP v2, PEAP-TLS o EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Agregar el servidor de acceso de red como un cliente RADIUS en NPS

Utilice este procedimiento para agregar un servidor de acceso de red como un cliente RADIUS en NPS. Puede usar este procedimiento para configurar un servidor NAS como cliente RADIUS mediante la consola de NPS.

Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para agregar un servidor de acceso de red como un cliente RADIUS en NPS

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.
2. En la consola NPS, haga doble clic en **clientes y servidores RADIUS**. Haga clic en **clientes RADIUS**y, a continuación, haga clic en **nuevo cliente RADIUS**. 
3. En **nuevo cliente RADIUS**, compruebe que la **habilitar este cliente RADIUS** casilla está activada.
4. En **nuevo cliente RADIUS**, en **Nombre_descriptivo**, escriba un nombre para mostrar de NAS. En **dirección (IP o DNS)**, escriba el nombre de dominio completo (FQDN) o la dirección IP de NAS. Si especifica el FQDN, haga clic en **compruebe** si desea comprobar que el nombre es correcto y se asigna a una dirección IP válida. 
5. En **nuevo cliente RADIUS**, en **proveedor**, especifique el nombre del fabricante NAS. Si no estás seguro de que el nombre del fabricante NAS, seleccione **estándar RADIUS**.
6. En **nuevo cliente RADIUS**, en **secreto compartido**, realice una de las siguientes acciones:
    - Asegúrese de que **Manual** está seleccionada y, a continuación, en **secreto compartido**, escriba la contraseña segura que también se especifica en el servidor NAS. Vuelva a escribir el secreto compartido en **Confirmar secreto compartido**.
    - Seleccione **generar**y, a continuación, haga clic en **generar** para generar automáticamente un secreto compartido. Guarde el secreto compartido generado para la configuración en el servidor NAS para que pueda comunicarse con NPS.
7. En **nuevo cliente RADIUS**, en **opciones adicionales**, si está utilizando los métodos de autenticación que no sean EAP y PEAP y si el servidor NAS admite el uso del atributo de autenticador de mensaje, seleccione **Mensajes de solicitud de acceso deben contener el atributo de autenticador de mensaje**.
8. Haga clic en **Aceptar**. El servidor NAS aparece en la lista de clientes RADIUS configurado en el NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurar a clientes RADIUS por intervalo de direcciones IP en Windows Server 2016 Datacenter

Si está ejecutando Windows Server 2016 Datacenter, puede configurar a clientes RADIUS en NPS por intervalo de direcciones IP. Esto le permite agregar un gran número de clientes RADIUS (por ejemplo, los puntos de acceso inalámbrico) en la consola NPS al mismo tiempo, en lugar de agregar individualmente cada cliente RADIUS.

No se puede configurar a clientes RADIUS por intervalo de direcciones IP si está ejecutando NPS en Windows Server 2016 Standard.

Utilice este procedimiento para agregar un grupo de servidores de acceso de red (NAS) como clientes RADIUS que se configuran con direcciones IP desde el mismo intervalo de direcciones IP.

Todos los clientes RADIUS en el intervalo deben usar la misma configuración y el secreto compartido.

Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Para configurar clientes RADIUS por intervalo de direcciones IP

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.
2. En la consola NPS, haga doble clic en **clientes y servidores RADIUS**. Haga clic en **clientes RADIUS**y, a continuación, haga clic en **nuevo cliente RADIUS**.
3. En **nuevo cliente RADIUS**, en **Nombre_descriptivo**, escriba un nombre para mostrar para la colección de NAS.
4. En **dirección \(IP o DNS\)**, escriba el intervalo de direcciones IP para los clientes RADIUS con el enrutamiento de interdominios sin clases \(CIDR\) notación. Por ejemplo, si el intervalo de direcciones IP para los servidores NAS es 10.10.0.0, escriba **10.10.0.0/16**.
5. En **nuevo cliente RADIUS**, en **proveedor**, especifique el nombre del fabricante NAS. Si no estás seguro de que el nombre del fabricante NAS, seleccione **estándar RADIUS**.
6. En **nuevo cliente RADIUS**, en **secreto compartido**, realice una de las siguientes acciones:
    - Asegúrese de que **Manual** está seleccionada y, a continuación, en **secreto compartido**, escriba la contraseña segura que también se especifica en el servidor NAS. Vuelva a escribir el secreto compartido en **Confirmar secreto compartido**.
    - Seleccione **generar**y, a continuación, haga clic en **generar** para generar automáticamente un secreto compartido. Guarde el secreto compartido generado para la configuración en el servidor NAS para que pueda comunicarse con NPS.
7. En **nuevo cliente RADIUS**, en **opciones adicionales**, si está utilizando los métodos de autenticación que no sean EAP y PEAP y si todos los servidores NAS admiten el uso del atributo de autenticador de mensaje, seleccione  **Mensajes de solicitud de acceso deben contener el atributo de autenticador de mensaje**.
8. Haga clic en **Aceptar**. Sus NAS aparecen en la lista de clientes RADIUS configurado en el NPS.

Para obtener más información, consulte [clientes RADIUS](nps-radius-clients.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).