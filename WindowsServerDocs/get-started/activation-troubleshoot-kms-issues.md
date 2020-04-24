---
title: Problemas conocidos de activación de KMS
description: Describe los problemas comunes que pueden producirse durante el proceso de activación de KMS, y proporciona soluciones e instrucciones.
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 3446ad0954510d8c96e9a2d361f24c90d325b782
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826258"
---
# <a name="kms-activation-known-issues"></a>Activación de KMS: problemas conocidos

En este artículo se describen preguntas y problemas comunes que pueden surgir durante las activaciones del Servicio de administración de claves (KMS) y se proporcionan instrucciones para solucionar los problemas.

> [!NOTE]
> Si sospechas que el problema está relacionado con DNS, consulta [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="should-i-back-up-kms-host-information"></a>¿Debería hacer una copia de seguridad de la información de host de KMS?

Las copias de seguridad no son necesarias para los hosts de KMS. Sin embargo, si usas una herramienta para limpiar periódicamente los registros de eventos, se puede perder el historial de activación almacenado en los registros. Si usas el registro de eventos para hacer un seguimiento de las activaciones de KMS o documentarlas, exporta periódicamente el registro de eventos del Servicio de administración de claves de la carpeta Registros de aplicaciones y servicios del Visor de eventos.

Si usas System Center Operations Manager, la base de datos de almacenamiento de datos de System Center almacena los datos del registro de eventos para generar informes, por lo que no es necesario hacer una copia de seguridad de los registros de eventos por separado.

## <a name="is-the-kms-client-computer-activated"></a>¿Está activado el equipo cliente de KMS?

En el equipo cliente de KMS, abre el panel de control del **Sistema** y busca el mensaje **Windows está activado**. Como alternativa, ejecuta Slmgr.vbs y usa la opción de línea de comandos **/dli**.

## <a name="the-kms-client-computer-does-not-activate"></a>El equipo cliente de KMS no se activa

Verifica si se cumple el umbral de activación de KMS. En el equipo host de KMS, ejecuta Slmgr.vbs y usa la opción de línea de comandos **/dli** para conocer el recuento actual del host. Hasta que el host de KMS tenga un recuento de 25, no se pueden activar los equipos cliente de Windows 7. Los clientes de KMS de Windows Server 2008 R2 requieren un recuento de KMS de 5 para la activación. Para obtener más información sobre los requisitos de KMS, consulta la [Guía de planeación de la activación por volumen](https://go.microsoft.com/fwlink/?linkid=155926). 

En el equipo cliente de KMS, busca el Id. de evento 12289 en el registro de eventos de aplicación. Comprueba este evento para obtener la información siguiente:

- ¿Es **0** el código de resultado? Todo lo demás es un error.
- ¿Es correcto el nombre de host de KMS en el evento?
- ¿Es correcto el puerto KMS?
- ¿Es accesible el host de KMS?
- Si el cliente ejecuta un firewall que no es de Microsoft, ¿se debe configurar el puerto de salida?

En el equipo cliente de KMS, busca el Id. de evento 12290 en el registro de eventos de KMS. Comprueba este evento para obtener la información siguiente:

- ¿Registró el host de KMS una solicitud del equipo cliente? Comprueba que aparece el nombre del equipo cliente de KMS. Comprueba que el cliente y el host de KMS puedan comunicarse. ¿Recibió el cliente la respuesta?
- Si no se registra ningún evento desde el cliente de KMS, la solicitud no llegó al host de KMS, o el host de KMS no pudo procesarlo. Asegúrate de que los enrutadores no bloqueen el tráfico usando el puerto TCP 1688 (si se usa el puerto predeterminado) y de que se permita el tráfico con estado al cliente de KMS.

## <a name="what-does-this-error-code-mean"></a>¿Qué significa este código de error?

A excepción de los eventos de KMS que tienen el Id. de evento 12290, Windows registra todos los eventos de activación en el registro de eventos de aplicación en el nombre de proveedor de eventos Microsoft-Windows-Security-SPP. Windows registra los eventos de KMS en el registro del Servicio de administración de claves en la carpeta Aplicaciones y servicios. Los profesionales de TI pueden ejecutar Slui.exe para mostrar una descripción de la mayoría de los códigos de error relacionados con la activación. La sintaxis general de este comando es la siguiente:

```cmd
slui.exe 0x2a ErrorCode
```

Por ejemplo, si el Id. de evento 12293 contiene el código de error 0x8007267C, puedes mostrar una descripción del error mediante la ejecución del siguiente comando:

```cmd
slui.exe 0x2a 0x8007267C
```

Para obtener más información sobre los códigos de error específicos y cómo solucionarlos, consulta [Resolución de problemas de códigos de error de activación](activation-error-codes.md).

## <a name="clients-are-not-adding-to-the-kms-count"></a>Los clientes no suman al recuento de KMS

Para restablecer el id. del equipo cliente (CMID) y otra información de activación del producto, ejecuta **sysprep /generalize** o **slmgr /rearm**. De lo contrario, cada equipo cliente tiene un aspecto idéntico, y el host de KMS no los cuenta como clientes de KMS independientes.

## <a name="kms-hosts-are-unable-to-create-srv-records"></a>Los hosts de KMS no pueden crear registros SRV

El sistema de nombres de dominio (DNS) puede restringir el acceso de escritura o puede que no admita DNS dinámico (DDNS). En este caso, concede al host de KMS acceso de escritura a la base de datos de DNS o crea manualmente el registro de recursos (RR) de servicio (SRV). Para obtener más información acerca de los problemas de KMS y DNS, consulta [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="only-the-first-kms-host-is-able-to-create-srv-records"></a>Solo el primer host de KMS puede crear registros SRV

Si la organización tiene más de un host de KMS, es posible que los demás hosts no puedan actualizar el RR de SRV a menos que se cambien los permisos predeterminados de SRV. Para obtener más información acerca de los problemas de KMS y DNS, consulta [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="i-installed-a-kms-key-on-the-kms-client"></a>He instalado una clave de KMS en el cliente de KMS

Las claves de KMS deben instalarse solo en hosts de KMS, no en clientes de KMS. Ejecuta **slmgr.vbs -ipk &lt;ClaveDeInstalación&gt;** . Para obtener las tablas de claves que puedes usar para configurar el equipo como cliente de KMS, consulta [Claves de configuración del cliente KMS](KMSclientkeys.md). Estas claves se conocen públicamente y son específicas de la edición. No olvides eliminar los RR de SRV innecesarios de DNS y, luego, reinicia los equipos.

## <a name="a-kms-host-failed"></a>Host de KMS erróneo

Si se produce un error en un host de KMS, debes instalar una clave de host de KMS en un nuevo host y, luego, activar el host. Asegúrate de que el nuevo host de KMS tenga un RR de SRV en la base de datos de DNS. Si instalas el nuevo host de KMS con el mismo nombre de equipo y la misma dirección IP que el host de KMS erróneo, el nuevo host de KMS puede usar el registro SRV de DNS del host erróneo. Si el nuevo host tiene un nombre de equipo diferente, puedes quitar manualmente el RR de SRV de DNS del host erróneo o (si la limpieza está habilitada en DNS) permitir que DNS lo quite automáticamente. Si la red usa DDNS, el nuevo host de KMS crea automáticamente un nuevo RR de SRV en el servidor DNS. A continuación, el nuevo host de KMS comienza a recopilar las solicitudes de renovación de clientes y comienza a activar los clientes en cuanto se alcanza el umbral de activación de KMS.

Si los clientes de KMS usan la detección automática, seleccionan automáticamente otro host de KMS si el host de KMS original no responde a las solicitudes de renovación. Si los clientes no usan la detección automática, debes actualizar manualmente los equipos cliente de KMS que se asignaron al host de KMS erróneo mediante la ejecución de **slmgr.vbs /skms**. Para evitar este escenario, configura los clientes de KMS para que usen la detección automática. Para obtener más información, consulte la [Guía de implementación de activación de licencias por volumen](https://go.microsoft.com/fwlink/?linkid=150083).
