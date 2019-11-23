---
title: Configurar clientes RADIUS
description: En este tema se proporciona información acerca de la configuración de clientes RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6870029e02ae91b1ef5bf4d4302ac2bed2e27d84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405295"
---
# <a name="configure-radius-clients"></a>Configurar clientes RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para configurar los servidores de acceso a la red como clientes RADIUS en NPS.

Al agregar un nuevo servidor de acceso a la red \(servidor VPN, un punto de acceso inalámbrico, un conmutador de autenticación o un servidor de acceso telefónico\) a la red, debe agregar el servidor como un cliente RADIUS en NPS y, a continuación, configurar el cliente RADIUS para comunicarse con el NPS.

>[!IMPORTANT]
>Los equipos cliente y dispositivos, como equipos portátiles, tabletas, teléfonos y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso a la red, como puntos de acceso inalámbricos, conmutadores compatibles con 802.1 X, servidores de red privada virtual (VPN) y servidores de acceso telefónico, porque usan el protocolo RADIUS para comunicarse con servidores RADIUS, como servidor de directivas de redes \(servidores NPS\).

Este paso también es necesario cuando el NPS es miembro de un grupo de servidores RADIUS remotos que está configurado en un proxy NPS. En este caso, además de realizar los pasos de esta tarea en el proxy NPS, debe hacer lo siguiente:

- En el proxy NPS, configure un grupo de servidores RADIUS remoto que contenga el NPS.
- En el NPS remoto, configure el proxy NPS como un cliente RADIUS.

Para realizar los procedimientos de este tema, debe tener al menos un servidor de acceso a la red \(servidor VPN, un punto de acceso inalámbrico, un conmutador de autenticación o un servidor de acceso telefónico\) o un proxy NPS físicamente instalado en la red.

## <a name="configure-the-network-access-server"></a>Configurar el servidor de acceso a la red

Use este procedimiento para configurar servidores de acceso a la red para su uso con NPS. Al implementar servidores de acceso a la red (NAS) como clientes RADIUS, debe configurar los clientes para que se comuniquen con el NPSs en el que los NAS están configurados como clientes.

Este procedimiento proporciona directrices generales sobre la configuración que debe usar para configurar los NAS; para obtener instrucciones específicas sobre cómo configurar el dispositivo que se va a implementar en la red, consulte la documentación del producto de NAS.

### <a name="to-configure-the-network-access-server"></a>Para configurar el servidor de acceso a la red

1. En el servidor NAS, en **configuración de RADIUS**, seleccione **autenticación RADIUS** en el puerto **1812** del Protocolo de datagramas de usuario (UDP) y **Contabilidad RADIUS** en el puerto UDP **1813**.
2. En **servidor de autenticación** o **servidor RADIUS**, especifique el NPS por dirección IP o nombre de dominio completo (FQDN), dependiendo de los requisitos del NAS. 
3. En **secreto** o **secreto compartido**, escriba una contraseña segura. Al configurar el NAS como cliente RADIUS en NPS, usará la misma contraseña, por lo que no lo olvide.
4. Si usa PEAP o EAP como método de autenticación, configure el NAS para usar la autenticación EAP.
5. Si está configurando un punto de acceso inalámbrico, en **SSID**, especifique un identificador de conjunto de servicios \(SSID\), que es una cadena alfanumérica que actúa como nombre de red. Este nombre lo emiten los puntos de acceso a los clientes inalámbricos y es visible para los usuarios en las zonas Wi-Fi inalámbricas \(\).
6. Si está configurando un punto de acceso inalámbrico, en **802.1 x y WPA**, habilite la **autenticación IEEE 802.1 x** si desea implementar PEAP-MS-CHAP V2, PEAP-TLS o EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Agregar el servidor de acceso a la red como un cliente RADIUS en NPS

Use este procedimiento para agregar un servidor de acceso a la red como cliente RADIUS en NPS. Puede usar este procedimiento para configurar un servidor NAS como cliente RADIUS mediante la consola NPS.

Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para agregar un servidor de acceso a la red como cliente RADIUS en NPS

1. En el NPS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.
2. En la consola de NPS, haga doble clic en **clientes y servidores RADIUS**. Haga clic con el botón secundario en **clientes RADIUS**y, a continuación, haga clic en **nuevo cliente RADIUS**. 
3. En **nuevo cliente RADIUS**, compruebe que la casilla **habilitar este cliente RADIUS** está activada.
4. En **nuevo cliente RADIUS**, en **nombre descriptivo**, escriba un nombre para mostrar para el NAS. En **dirección (IP o DNS)** , escriba la dirección IP de NAS o el nombre de dominio completo (FQDN). Si escribe el FQDN, haga clic en **comprobar** si desea comprobar que el nombre es correcto y se asigna a una dirección IP válida. 
5. En **nuevo cliente RADIUS**, en **proveedor**, especifique el nombre del fabricante de NAS. Si no está seguro del nombre del fabricante de NAS, seleccione **RADIUS estándar**.
6. En **nuevo cliente RADIUS**, en **secreto compartido**, realice una de las acciones siguientes:
    - Asegúrese de que la opción **manual** está seleccionada y, a continuación, en **secreto compartido**, escriba la contraseña segura que se ha escrito también en el NAS. Vuelva a escribir el secreto compartido en **confirmar secreto compartido**.
    - Seleccione **generar**y, a continuación, haga clic en **generar** para generar automáticamente un secreto compartido. Guarde el secreto compartido generado para la configuración en el servidor NAS para que pueda comunicarse con el NPS.
7. En el **nuevo cliente RADIUS**, en **opciones adicionales**, si usa métodos de autenticación que no sean EAP y PEAP, y si el NAS admite el uso del atributo de autenticador de mensaje, seleccione **los mensajes de solicitud de acceso deben contener el atributo de autenticador de mensaje**.
8. Haz clic en **Aceptar**. El NAS aparece en la lista de clientes RADIUS configurados en el NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurar clientes RADIUS por intervalo de direcciones IP en Windows Server 2016 Datacenter

Si está ejecutando Windows Server 2016 Datacenter, puede configurar clientes RADIUS en NPS por intervalo de direcciones IP. Esto le permite agregar un gran número de clientes RADIUS (como puntos de acceso inalámbrico) a la consola de NPS al mismo tiempo, en lugar de agregar cada cliente RADIUS individualmente.

No se pueden configurar clientes RADIUS por intervalo de direcciones IP si se ejecuta NPS en Windows Server 2016 Standard.

Utilice este procedimiento para agregar un grupo de servidores de acceso a la red (NAS) como clientes RADIUS que están configurados con direcciones IP del mismo intervalo de direcciones IP.

Todos los clientes RADIUS del intervalo deben usar la misma configuración y secreto compartido.

Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Para configurar clientes RADIUS por intervalo de direcciones IP

1. En el NPS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.
2. En la consola de NPS, haga doble clic en **clientes y servidores RADIUS**. Haga clic con el botón secundario en **clientes RADIUS**y, a continuación, haga clic en **nuevo cliente RADIUS**.
3. En **nuevo cliente RADIUS**, en **nombre descriptivo**, escriba un nombre para mostrar para la colección de NAS.
4. En **dirección \(\)IP o DNS** , escriba el intervalo de direcciones IP para los clientes RADIUS mediante el enrutamiento de interdominios sin clases \(notación de\) CIDR. Por ejemplo, si el intervalo de direcciones IP de los NAS es 10.10.0.0, escriba **10.10.0.0/16**.
5. En **nuevo cliente RADIUS**, en **proveedor**, especifique el nombre del fabricante de NAS. Si no está seguro del nombre del fabricante de NAS, seleccione **RADIUS estándar**.
6. En **nuevo cliente RADIUS**, en **secreto compartido**, realice una de las acciones siguientes:
    - Asegúrese de que la opción **manual** está seleccionada y, a continuación, en **secreto compartido**, escriba la contraseña segura que se ha escrito también en el NAS. Vuelva a escribir el secreto compartido en **confirmar secreto compartido**.
    - Seleccione **generar**y, a continuación, haga clic en **generar** para generar automáticamente un secreto compartido. Guarde el secreto compartido generado para la configuración en el servidor NAS para que pueda comunicarse con el NPS.
7. En el **nuevo cliente RADIUS**, en **opciones adicionales**, si usa métodos de autenticación que no sean EAP y PEAP, y si todos los NAS admiten el uso del atributo de autenticador de mensaje, seleccione **los mensajes de solicitud de acceso deben contener el atributo de autenticador de mensaje**.
8. Haz clic en **Aceptar**. Los NAS aparecen en la lista de clientes RADIUS configurados en el NPS.

Para obtener más información, consulte [clientes RADIUS](nps-radius-clients.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).