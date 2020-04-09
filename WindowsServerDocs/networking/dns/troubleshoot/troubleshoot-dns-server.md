---
title: Solución de problemas de servidores DNS
description: En este artículo se explica cómo solucionar problemas de DNS desde el lado servidor.
manager: dcscontentpm
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 4413c60072c43b623f386d5037e3da7ed5dc128d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861328"
---
# <a name="troubleshooting-dns-servers"></a>Solución de problemas de servidores DNS

En este artículo se describe cómo solucionar problemas en los servidores DNS.

## <a name="check-ip-configuration"></a>Comprobar la configuración de IP

1. Ejecute `ipconfig /all` en un símbolo del sistema y Compruebe la dirección IP, la máscara de subred y la puerta de enlace predeterminada.

2. Compruebe si el servidor DNS es autoritativo para el nombre que se está buscando. Si es así, consulte [comprobación de problemas con datos autoritativos](#checking-for-problems-with-authoritative-data).

3. Ejecuta el siguiente comando:

   ```cmd
   nslookup <name> <IP address of the DNS server>
   ```
   Por ejemplo: 
   ```cmd
   nslookup app1 10.0.0.1
   ```
   Si se produce un error o una respuesta de tiempo de espera, consulte [comprobar si hay problemas de recursividad](#checking-for-recursion-problems).

4. Vacíe la memoria caché de la resolución. Para ello, ejecute el siguiente comando en una ventana de símbolo del sistema administrativo:

   ```cmd
   dnscmd /clearcache
   ```
   O bien, en una ventana administrativa de PowerShell, ejecute el siguiente cmdlet:
   ```powershell
   Clear-DnsServerCache
   ```

5. Repita el paso 3. 

## <a name="check-dns-server-problems"></a>Comprobar problemas del servidor DNS

### <a name="event-log"></a>Registro de eventos

Compruebe los registros siguientes para ver si hay errores registrados:

- Aplicación

- System

- DNS Server

### <a name="test-by-using-nslookup-query"></a>Prueba mediante la consulta nslookup

Ejecute el siguiente comando y compruebe si el servidor DNS es accesible desde los equipos cliente.

```cmd
nslookup <client name> <server IP address>
```

- Si el solucionador devuelve la dirección IP del cliente, el servidor no tiene problemas.

- Si el solucionador devuelve una respuesta "error del servidor" o "consulta rechazada", es probable que la zona esté en pausa o que el servidor esté sobrecargado. Para saber si está en pausa, Compruebe la pestaña General de las propiedades de la zona en la consola DNS.

Si el solucionador devuelve una respuesta "se agotó el tiempo de espera de la solicitud al servidor" o "no hay respuesta del servidor", es probable que el servicio DNS no se esté ejecutando. Intente reiniciar el servicio servidor DNS; para ello, escriba lo siguiente en un símbolo del sistema en el servidor:

```cmd
net start DNS
```

Si el problema se produce cuando el servicio se está ejecutando, es posible que el servidor no esté escuchando en la dirección IP que usó en la consulta nslookup. En la pestaña **interfaces** de la página Propiedades del servidor de la consola DNS, los administradores pueden restringir un servidor DNS para que escuche solo en las direcciones seleccionadas. Si el servidor DNS se ha configurado para limitar el servicio a una lista específica de sus direcciones IP configuradas, es posible que la dirección IP que se usa para ponerse en contacto con el servidor DNS no esté en la lista. Puede probar una dirección IP diferente en la lista o agregar la dirección IP a la lista.

En raras ocasiones, es posible que el servidor DNS tenga una configuración avanzada de seguridad o firewall. Si el servidor se encuentra en otra red a la que solo se puede tener acceso a través de un host intermedio (como un enrutador de filtrado de paquetes o un servidor proxy), el servidor DNS puede usar un puerto no estándar para escuchar y recibir solicitudes de cliente. De forma predeterminada, nslookup envía consultas a los servidores DNS en el puerto UDP 53. Por lo tanto, si el servidor DNS usa cualquier otro puerto, las consultas nslookup producen un error. Si cree que esto podría ser el problema, compruebe si un filtro intermedio se usa intencionadamente para bloquear el tráfico en los puertos DNS conocidos. Si no es así, intente modificar los filtros de paquetes o las reglas de puerto en el firewall para permitir el tráfico en el puerto UDP/TCP 53.

## <a name="checking-for-problems-with-authoritative-data"></a>Comprobación de problemas con datos autoritativos

Compruebe si el servidor que devuelve la respuesta incorrecta es un servidor principal de la zona (el servidor principal estándar de la zona o un servidor que usa la integración de Active Directory para cargar la zona) o un servidor que hospeda una copia secundaria de la zona. 

### <a name="if-the-server-is-a-primary-server"></a>Si el servidor es un servidor principal

El problema puede deberse a un error del usuario cuando los usuarios escriben datos en la zona. O bien, puede deberse a un problema que afecte a la replicación de Active Directory o a la actualización dinámica.

### <a name="if-the-server-is-hosting-a-secondary-copy-of-the-zone"></a>Si el servidor hospeda una copia secundaria de la zona

1. Examine la zona en el servidor maestro (el servidor desde el que este servidor extrae las transferencias de zona).

   > [!NOTE]
   >Puede determinar qué servidor es el servidor maestro mediante el examen de las propiedades de la zona secundaria en la consola DNS.

   Si el nombre no es correcto en el servidor maestro, vaya al paso 4.

2. Si el nombre es correcto en el servidor maestro, compruebe si el número de serie del servidor maestro es menor o igual que el número de serie en el servidor secundario. Si es así, modifique el servidor maestro o el servidor secundario para que el número de serie del servidor maestro sea mayor que el número de serie en el servidor secundario. 
  
3. En el servidor secundario, fuerce una transferencia de zona desde la consola DNS o mediante la ejecución del siguiente comando:
  
   ```cmd
   dnscmd /zonerefresh <zone name>
   ```
  
   Por ejemplo, si la zona es corp.contoso.com, escriba: `dnscmd /zonerefresh corp.contoso.com`.
  
4. Vuelva a examinar el servidor secundario para ver si la zona se transfirió correctamente. Si no es así, es probable que tenga un problema de transferencia de zona. Para obtener más información, consulte [problemas de transferencia de zona](#zone-transfer-problems).

5. Si la zona se transfirió correctamente, compruebe si los datos son correctos. Si no es así, los datos son incorrectos en la zona principal. El problema puede deberse a un error del usuario cuando los usuarios escriben datos en la zona. O bien, puede deberse a un problema que afecte a la replicación de Active Directory o a la actualización dinámica.

## <a name="checking-for-recursion-problems"></a>Comprobando si hay problemas de recursividad

Para que la recursividad funcione correctamente, todos los servidores DNS que se usan en la ruta de acceso de una consulta recursiva deben ser capaces de responder y reenviar los datos correctos. Si no es así, se puede producir un error en una consulta recursiva por cualquiera de los siguientes motivos:

- La consulta agota el tiempo de espera antes de que se pueda completar.

- Un servidor que se usa durante la consulta no responde.

- Un servidor que se usa durante la consulta proporciona datos incorrectos.

Inicie la solución de problemas en el servidor que se usó en la consulta original. Compruebe si este servidor reenvía consultas a otro servidor mediante el examen de la ficha **reenviadores** en las propiedades del servidor en la consola DNS. Si la casilla **Habilitar reenviadores** está activada y se enumeran uno o varios servidores, este servidor reenvía las consultas.

Si este servidor reenvía consultas a otro servidor, compruebe si hay problemas que afecten al servidor al que este servidor reenvía consultas. Para comprobar si hay problemas, consulte [comprobación de problemas del servidor DNS](#check-dns-server-problems). Cuando esa sección le indica que realice una tarea en el cliente, realice la ejecución en el servidor en su lugar.

Si el servidor está en buen estado y puede reenviar consultas, repita este paso y examine el servidor al que este servidor reenvía las consultas.

Si este servidor no reenvía consultas a otro servidor, compruebe si este servidor puede consultar un servidor raíz. Para ello, ejecute el comando siguiente:

```cmd
nslookup
server <IP address of server being examined>
set q=NS
 ```

- Si el solucionador devuelve la dirección IP de un servidor raíz, es probable que tenga una delegación rota entre el servidor raíz y el nombre o la dirección IP que está intentando resolver. Siga el procedimiento [probar una delegación rota](#test-a-broken-delegation) para determinar dónde tiene una delegación rota.

- Si el solucionador devuelve una respuesta "se agotó el tiempo de espera del servidor de la solicitud", compruebe si las sugerencias de raíz señalan a los servidores raíz en funcionamiento. Para ello, utilice [para ver el procedimiento de sugerencias de raíz actual](#to-view-the-current-root-hints) . Si las sugerencias de raíz señalan a servidores raíz que funcionan, es posible que se deba a un problema de red o que el servidor use una configuración avanzada de firewall que impida que el solucionador realice una consulta al servidor, como se describe en la sección comprobación de los [problemas del servidor DNS](#check-dns-server-problems) . También es posible que el valor predeterminado de tiempo de espera recursivo sea demasiado corto.

### <a name="test-a-broken-delegation"></a>Probar una delegación rota

Comience las pruebas en el procedimiento siguiente consultando un servidor raíz válido. La prueba le guía a través de un proceso de consulta de todos los servidores DNS desde la raíz hasta el servidor en el que está probando una delegación rota.

1. En el símbolo del sistema del servidor que está probando, escriba lo siguiente:

   ```cmd
   nslookup
   server <server IP address>
   set norecursion
   set querytype= <resource record type>
   <FQDN>
   ```
   > [!NOTE]
   >Tipo de registro de recursos es el tipo de registro de recursos que se estaba consultando en la consulta original y FQDN es el FQDN para el que se estaba consultando (finalizado en un punto).
 
2. Si la respuesta incluye una lista de registros de recursos "NS" y "A" para los servidores delegados, repita el paso 1 para cada servidor y use la dirección IP de los registros de recursos "A" como dirección IP del servidor.

   - Si la respuesta no contiene un registro de recursos "NS", tiene una delegación rota.
   
   - Si la respuesta contiene registros de recursos "NS", pero no registros de recursos "A", escriba **recursividad de conjunto**y consulte individualmente los registros de recursos de "a" de los servidores que se enumeran en los registros "NS". Si no encuentra al menos una dirección IP válida de un registro de recursos "A" para cada registro de recursos NS de una zona, tiene una delegación rota.

3. Si determina que tiene una delegación rota, corríjalo agregando o actualizando un registro de recursos "A" en la zona principal mediante una dirección IP válida para un servidor DNS correcto para la zona delegada.

### <a name="to-view-the-current-root-hints"></a>Para ver las sugerencias de raíz actuales

1. Inicie la consola DNS.

2. Agregue o conéctese al servidor DNS en el que se produjo un error en una consulta recursiva.

3. Haga clic con el botón secundario en el servidor y seleccione **propiedades**.

4. Haga clic en sugerencias de raíz.

Compruebe la conectividad básica de los servidores raíz.

- Si parece que las sugerencias de raíz están configuradas correctamente, compruebe que el servidor DNS que se usa en una resolución de nombres con error puede hacer ping a los servidores raíz por dirección IP.

- Si los servidores raíz no responden a la ping por dirección IP, es posible que las direcciones IP de los servidores raíz hayan cambiado. Sin embargo, no es habitual ver una reconfiguración de los servidores raíz.

## <a name="zone-transfer-problems"></a>Problemas de transferencia de zona

Ejecute las siguientes comprobaciones:

- Compruebe Visor de eventos para el servidor DNS principal y secundario.

- Compruebe el servidor maestro para ver si se rechaza el envío de la transferencia por seguridad. 

- Compruebe la ficha **transferencias de zona** de las propiedades de la zona en la consola DNS. Si el servidor restringe las transferencias de zona a una lista de servidores, como los que aparecen en la pestaña **servidores de nombres** de las propiedades de la zona, asegúrese de que el servidor secundario se encuentra en esa lista. Asegúrese de que el servidor está configurado para enviar transferencias de zona.

- Para comprobar si hay problemas en el servidor principal, siga los pasos de la sección comprobación de los [problemas del servidor DNS](#check-dns-server-problems) . Cuando se le pida que realice una tarea en el cliente, realice la tarea en el servidor secundario en su lugar.

- Compruebe si el servidor secundario está ejecutando otra implementación de servidor DNS, como BIND. Si es así, el problema puede deberse a una de las siguientes causas:

  - Es posible que el servidor maestro de Windows esté configurado para enviar transferencias de zona rápidas, pero es posible que el servidor secundario de terceros no admita las transferencias de zona rápida. En este caso, deshabilite las transferencias de zona rápida en el servidor maestro desde la consola DNS activando la casilla **Habilitar secundarios de enlace** en la ficha **Opciones avanzadas** de las propiedades del servidor.

  - Si una zona de búsqueda directa en el servidor de Windows contiene un tipo de registro (por ejemplo, un registro SRV) que no admite el servidor secundario, es posible que el servidor secundario tenga problemas al extraer la zona.

Compruebe si el servidor maestro está ejecutando otra implementación de servidor DNS, como BIND. Si es así, es posible que la zona del servidor maestro incluya registros de recursos incompatibles que Windows no reconoce.
 
Si el servidor principal o secundario está ejecutando otra implementación de servidor DNS, compruebe ambos servidores para asegurarse de que admiten las mismas características. Puede comprobar el servidor de Windows en la consola DNS de la ficha **Opciones avanzadas** de la página de propiedades del servidor. Además del cuadro habilitar secundarios de enlace, esta página incluye la lista desplegable **comprobación de nombres** . Esto le permite seleccionar la aplicación de la compatibilidad estricta con RFC para los caracteres en nombres DNS.

