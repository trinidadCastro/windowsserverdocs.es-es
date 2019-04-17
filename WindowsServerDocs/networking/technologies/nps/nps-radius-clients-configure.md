---
title: Configurar a los clientes RADIUS
description: Este tema proporciona información sobre la configuración de clientes RADIUS de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9af3189199c8282394ca34f181a90b4290c55044
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-radius-clients"></a>Configurar a los clientes RADIUS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para configurar los servidores de acceso de red como clientes RADIUS en NPS.

Cuando agregas un servidor de acceso de red nueva \ (servidor VPN, punto de acceso inalámbrico, conmutador de autenticación o telefónico server\) a la red, debes agregar el servidor como un cliente RADIUS en NPS y, a continuación, configura el cliente RADIUS para comunicarse con el servidor NPS.

>[!IMPORTANT]
>Los equipos cliente y dispositivos, como equipos portátiles, tabletas, teléfonos y otros equipos que ejecutan sistemas operativos cliente, no son clientes de RADIUS. Clientes de RADIUS son servidores de acceso de red - como puntos de acceso inalámbrico, conmutadores compatibles con X de 802.1X, privada virtual de red, los servidores (VPN) y servidores de acceso telefónico, ya que usan el protocolo RADIUS para comunicarse con los servidores RADIUS, como los servidores de servidor de directivas de red \(NPS\).

Este paso también es necesario cuando el servidor NPS es un miembro de un grupo de servidores RADIUS remoto configurado en un servidor proxy de NPS. En este caso, además de realizar los pasos de esta tarea en el servidor proxy NPS, haz lo siguiente:

- En el servidor proxy NPS, configurar un grupo de servidores RADIUS remotos que contiene el servidor NPS.
- En el servidor NPS remoto, configurar al proxy NPS como un cliente RADIUS.

Para realizar los procedimientos descritos en este tema, debes tener al menos un servidor de acceso de red \ (servidor VPN, punto de acceso inalámbrico, conmutador de autenticación o telefónico server\) o proxy NPS físicamente instalados en la red.

## <a name="configure-the-network-access-server"></a>Configurar el servidor de acceso de red

Usa este procedimiento para configurar los servidores de acceso de red para su uso con NPS. Cuando implementas servidores de acceso de red (NAS) como clientes RADIUS, debes configurar los clientes para comunicarse con los servidores NPS donde los NAS están configurados como clientes.

Este procedimiento proporciona directrices generales sobre la configuración que se debe usar para configurar sus NAS; Para obtener instrucciones específicas sobre cómo configurar el dispositivo que se va a implementar en la red, consulta la documentación del producto NAS.

### <a name="to-configure-the-network-access-server"></a>Para configurar el servidor de acceso de red

1. En el servidor de NAS en **configuración RADIUS**, selecciona **autenticación RADIUS** en el puerto de protocolo de datagramas de usuario (UDP) **1812** y **cuentas RADIUS** en el puerto UDP **1813**.
2. En **servidor de autenticación** o **RADIUS server**, especificar el servidor NPS por dirección IP o el nombre de dominio completo (FQDN), según los requisitos de NAS. 
3. En **secreto** o **secreto compartido**, escribe una contraseña segura. Cuando se configura el NAS como un cliente RADIUS en NPS, deberás utilizar la misma contraseña, por lo que no olvidas.
4. Si usas PEAP o EAP como método de autenticación, configurar NAS para usar la autenticación de EAP.
5. Si estás configurando un punto de acceso inalámbrico, en **SSID**, especifica un identificador de conjunto de servicio \(SSID\), que es una cadena alfanumérica que actúa como el nombre de red. Este nombre se difundida por los puntos de acceso a los clientes inalámbricos y es visible para los usuarios de las zonas interactivas \(Wi-Fi\) de fidelidad inalámbrica.
6. Si estás configurando un punto de acceso inalámbrico, en **802.1X y WPA**, habilitar **autenticación IEEE 802.1X** si quieres implementar PEAP-MS-CHAP v2, PEAP-TLS y EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Agregar el servidor de acceso de red como un cliente RADIUS en NPS

Usa este procedimiento para agregar un servidor de acceso de red como un cliente RADIUS en NPS. Puedes usar este procedimiento para configurar un NAS como un cliente RADIUS mediante la consola NPS.

Para completar este procedimiento, debe ser miembro de la **administradores** grupo.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para agregar un servidor de acceso de red como un cliente RADIUS en NPS

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.
2. En la consola NPS, haz doble clic en **clientes y servidores RADIUS**. Haz clic en **clientes RADIUS**y, a continuación, haz clic en **nuevo cliente RADIUS**. 
3. En **nuevo cliente RADIUS**, comprueba que la **habilitar este cliente RADIUS** casilla está activada.
4. En **nuevo cliente RADIUS**, en **nombre descriptivo**, escribe un nombre para mostrar de NAS. En **dirección (IP o DNS)**, escribe la dirección IP de NAS o el nombre de dominio completo (FQDN). Si escribes el FQDN, haz clic en **comprobar** si desea comprobar que el nombre es correcto y asigna a una dirección IP válida. 
5. En **nuevo cliente RADIUS**, en **proveedor**, especificar el nombre del fabricante NAS. Si no estás seguro de que el nombre del fabricante NAS, selecciona **estándar RADIUS**.
6. En **nuevo cliente RADIUS**, en **secreto compartido**, realiza una de las siguientes acciones:
    - Asegúrate de que **Manual** está activada y, a continuación, en **secreto compartido**, escriba la contraseña segura que también se especifica en NAS. Vuelva a escribir la clave secreta compartida en **Confirmar secreto compartido**.
    - Selecciona **generar**y, a continuación, haz clic en **generar** para generar automáticamente un secreto compartido. Guardar el secreto compartido generado para la configuración en NAS por lo que se puede comunicar con el servidor NPS.
7. En **nuevo cliente RADIUS**, en **opciones adicionales**, si estás usando los métodos de autenticación que no sean EAP y PEAP y si el servidor NAS admite el uso del atributo de autenticación de mensaje, selecciona **mensajes de solicitud de acceso deben contener el atributo de autenticación de mensaje**.
8. Haz clic en **Aceptar**. NAS aparece en la lista de clientes RADIUS configurado en el servidor NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurar a los clientes RADIUS por intervalo de direcciones IP en el centro de datos de Windows Server 2016

Si estás ejecutando Windows Server 2016 Datacenter, puedes configurar a clientes RADIUS en NPS por intervalo de direcciones IP. Esto te permite agregar un gran número de clientes RADIUS (como puntos de acceso inalámbrico) en la consola NPS al mismo tiempo, en lugar de agregar a cada cliente RADIUS individualmente.

Si estás ejecutando NPS en Windows Server 2016 estándar no puede configurar a clientes RADIUS por intervalo de direcciones IP.

Usa este procedimiento para agregar un grupo de servidores de acceso de red (NAS) como los clientes RADIUS que están configurados con direcciones IP desde el mismo intervalo de direcciones IP.

Todos los clientes de RADIUS en el intervalo deben utilizar la misma configuración y secreto compartido.

Para completar este procedimiento, debe ser miembro de la **administradores** grupo.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Para configurar los clientes RADIUS por intervalo de direcciones IP

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.
2. En la consola NPS, haz doble clic en **clientes y servidores RADIUS**. Haz clic en **clientes RADIUS**y, a continuación, haz clic en **nuevo cliente RADIUS**.
3. En **nuevo cliente RADIUS**, en **nombre descriptivo**, escribe un nombre para mostrar de la colección de NAS.
4. En **dirección \(IP or DNS\)**, escriba el intervalo de direcciones IP para los clientes RADIUS mediante la notación de enrutamiento de interdominios sin clases \(CIDR\). Por ejemplo, si el intervalo de direcciones IP para los NAS es 10.10.0.0, escriba **10.10.0.0/16**.
5. En **nuevo cliente RADIUS**, en **proveedor**, especificar el nombre del fabricante NAS. Si no estás seguro de que el nombre del fabricante NAS, selecciona **estándar RADIUS**.
6. En **nuevo cliente RADIUS**, en **secreto compartido**, realiza una de las siguientes acciones:
    - Asegúrate de que **Manual** está activada y, a continuación, en **secreto compartido**, escriba la contraseña segura que también se especifica en NAS. Vuelva a escribir la clave secreta compartida en **Confirmar secreto compartido**.
    - Selecciona **generar**y, a continuación, haz clic en **generar** para generar automáticamente un secreto compartido. Guardar el secreto compartido generado para la configuración en NAS por lo que se puede comunicar con el servidor NPS.
7. En **nuevo cliente RADIUS**, en **opciones adicionales**, si estás usando los métodos de autenticación que no sean EAP y PEAP y si todas sus NAS admiten el uso del atributo de autenticación de mensaje, selecciona **mensajes de solicitud de acceso deben contener el atributo de autenticación de mensaje**.
8. Haz clic en **Aceptar**. Sus NAS aparecen en la lista de clientes RADIUS configurado en el servidor NPS.

Para obtener más información, consulta [clientes RADIUS](nps-radius-clients.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).