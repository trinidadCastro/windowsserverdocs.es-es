---
title: Solución de problemas de clientes DNS
description: En este artículo se explica cómo solucionar problemas de DNS desde el lado cliente.
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 8098f49c0a48004c54e4acc67522d61d4b30717c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958643"
---
# <a name="troubleshooting-dns-clients"></a>Solución de problemas de clientes DNS

En este artículo se describe cómo solucionar problemas de clientes DNS.

## <a name="check-ip-configuration"></a>Comprobar la configuración de IP

1. Abra una ventana del símbolo del sistema como administrador en el equipo cliente.

2. Ejecute el comando siguiente:

   ```cmd
   ipconfig /all
   ```

3. Compruebe que el cliente tiene una dirección IP, una máscara de subred y una puerta de enlace predeterminada válidas para la red a la que está conectado y que se está usando.

4. Compruebe los servidores DNS que aparecen en la salida y compruebe que las direcciones IP que se muestran son correctas.

5. Compruebe el sufijo DNS específico de la conexión en la salida y compruebe que es correcto.

Si el cliente no tiene una configuración de TCP/IP válida, use uno de los métodos siguientes:

* En el caso de los clientes configurados dinámicamente, use el `ipconfig /renew` comando para forzar manualmente al cliente a renovar la configuración de la dirección IP con el servidor DHCP.

* Para los clientes configurados estáticamente, modifique las propiedades de TCP/IP del cliente para usar parámetros de configuración válidos o complete su configuración de DNS para la red.

## <a name="check-network-connection"></a>Comprobar conexión de red

### <a name="ping-test"></a>Prueba de ping

Compruebe que el cliente puede ponerse en contacto con un servidor DNS preferido (o alternativo) haciendo ping en el servidor DNS preferido mediante su dirección IP.

Por ejemplo, si el cliente usa un servidor DNS preferido de 10.0.0.1, ejecute este comando en un símbolo del sistema:

```cmd
ping 10.0.0.1
```

Si ningún servidor DNS configurado responde a un ping directo de su dirección IP, esto indica que el origen del problema es la conectividad de red más probable entre el cliente y los servidores DNS. Si es así, siga los pasos básicos de solución de problemas de red TCP/IP para solucionar el problema. Tenga en cuenta que se debe permitir el tráfico ICMP a través del firewall para que el comando ping funcione.

### <a name="dns-query-tests"></a>Pruebas de consultas de DNS

Si el cliente DNS puede hacer ping en el equipo servidor DNS, intente usar los `nslookup` comandos siguientes para comprobar si el servidor puede responder a los clientes DNS. Dado que nslookup no utiliza la memoria caché de DNS del cliente, la resolución de nombres usará el servidor DNS configurado del cliente.

#### <a name="test-a-client"></a>Probar un cliente

```cmd
nslookup <client>
```

Por ejemplo, si el equipo cliente se llama **client1**, ejecute este comando:

```cmd
nslookup client1
```

Si no se devuelve una respuesta correcta, intente ejecutar el siguiente comando:

```cmd
nslookup <fqdn of client>
```

Por ejemplo, si el FQDN es **client1.Corp.contoso.com**, ejecute este comando:

```cmd
nslookup client1.corp.contoso.com.
```

> [!NOTE]
> Debe incluir el período final al ejecutar esta prueba.

Si Windows encuentra el FQDN correctamente, pero no encuentra el nombre corto, Compruebe la configuración de sufijo DNS en la pestaña DNS de la configuración avanzada de TCP/IP de la NIC. Para obtener más información, consulte Configuración de la [resolución de DNS](/previous-versions/tn-archive/dd163570(v=technet.10)#configuring-dns-resolution).

#### <a name="test-the-dns-server"></a>Prueba del servidor DNS

```cmd
nslookup <DNS Server>
```

Por ejemplo, si el servidor DNS se denomina DC1, ejecute este comando:

```cmd
nslookup dc1
```
Si las pruebas anteriores se realizaron correctamente, esta prueba también debe ser correcta. Si esta prueba no se realiza correctamente, Compruebe la conectividad con el servidor DNS.

#### <a name="test-the-failing-record"></a>Prueba del registro con errores

```cmd
nslookup <failed internal record>
```

Por ejemplo, si se **app1.Corp.contoso.com**el registro con errores, ejecute este comando:

```cmd
nslookup app1.corp.contoso.com
```

#### <a name="test-a-public-internet-address"></a>Prueba de una dirección de Internet pública

```cmd
nslookup <external name>
```

Por ejemplo:
```cmd
nslookup bing.com
```

Si las cuatro pruebas se realizaron correctamente, ejecute `ipconfig /displaydns` y compruebe el resultado del nombre en el que se produjo el error. Si ve el error "el nombre no existe" en el nombre con error, se devolvió una respuesta negativa desde un servidor DNS y se almacenó en caché en el cliente.

Para resolver el problema, borre la memoria caché. para ello, ejecute `ipconfig /flushdns` .

## <a name="next-step"></a>Paso siguiente

Si todavía se produce un error en la resolución de nombres, vaya a la sección [solución de problemas de servidores DNS](troubleshoot-dns-server.md) .
