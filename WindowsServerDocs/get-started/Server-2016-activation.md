---
title: Activación de Microsoft Server
description: Cómo activar Windows Server 2016.
ms.date: 09/19/2018
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: ad46c72b664bd1cb6b0a74e353d300dfd01e9d82
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959423"
---
# <a name="windows-server-2016-activation"></a>Activación de Windows Server 2016

La siguiente información describe las consideraciones de planeación iniciales que debes revisar para la activación de Servicios de administración de claves en relación con Windows Server 2016. Para obtener información sobre la activación de KMS en relación con los sistemas operativos anteriores a los que se indican aquí, consulta [Paso 1: Revisar y seleccionar métodos de activación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)).

KMS usa un modelo de cliente-servidor para activar los clientes. Los clientes de KMS se conectan a un servidor de KMS, denominado host de KMS, para la activación. El host de KMS debe residir en su red local.

No es necesario que los hosts de KMS sean servidores dedicados. KMS puede hospedarse con otros servicios. Puedes ejecutar un host de KMS en cualquier sistema físico o virtual que ejecute Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows 8.1 o Windows Server 2012.

Un host de KMS que se ejecuta en Windows 10 o Windows 8.1 solo puede activar equipos que ejecutan sistemas operativos cliente.
En la tabla siguiente se resumen los requisitos de host y de cliente de KMS para las redes que incluyen los clientes de Windows Server 2016 y Windows 10.

> [!NOTE]
> Puede que se necesite realizar actualizaciones del servidor KMS para permitir la activación de cualquiera de estos clientes más recientes. Si recibe errores de activación, compruebe que cuenta con las actualizaciones correspondientes que aparecen a continuación de esta tabla.

|Grupo de claves de producto|KMS se pueden hospedar en|Ediciones de Windows activadas por este host de KMS|
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
|Licencia por volumen para Windows Server 2016|Windows Server 2012<p>Windows Server 2012 R2<p>Windows Server 2016<p>|Canal semianual de Windows Server <br><br>Windows Server 2016 (todas las ediciones)<p>Windows 10 LTSB (2015 y 2016)<p>Windows 10 Professional<p>Windows 10 Enterprise<p>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br>Windows Server 2012 R2 (todas las ediciones)<p>Windows 8.1 Professional<p>Windows 8.1 Enterprise<p>Windows Server 2012 (todas las ediciones)<p>Windows Server 2008 R2 (todas las ediciones)<p>Windows Server 2008 (todas las ediciones)<p>Windows 7 Professional<p>Windows 7 Enterprise|
|Licencia por volumen para Windows 10|Windows 7<p>Windows 8.1<p> Windows 10|Windows 10 Professional<p> Windows 10 Professional N<p> Windows 10 Enterprise<p> Windows 10 Enterprise N<p> Windows 10 Education<p> Windows 10 Education N<p> Windows 10 Enterprise LTSB (2015)<p> Windows 10 Enterprise LTSB N (2015)<p> Windows 10 Pro for Workstations<br><br>Windows 8.1 Professional<p> Windows 8.1 Enterprise<p> Windows 7 Professional<p> Windows 7 Enterprise<p>|
|Licencia por volumen de Windows Server 2012 R2 para Windows 10|Windows Server 2008 R2<p> Windows Server 2012 Standard<p> Windows Server 2012 Datacenter<p> Windows Server 2012 R2 Standard<p>Windows Server 2012 R2 Datacenter|Windows 10 Professional<p> Windows 10 Enterprise<p>Windows 10 Enterprise LTSB (2015)<br><br>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br> Windows Server 2012 R2 (todas las ediciones)<p> Windows 8.1 Professional<p> Windows 8.1 Enterprise<p> Windows Server 2012 (todas las ediciones)<p> Windows Server 2008 R2 (todas las ediciones)<p>Windows Server 2008 (todas las ediciones)<p> Windows 7 Professional<p> Windows 7 Enterprise|

> [!NOTE]
> En función del sistema operativo que ejecuta el servidor KMS y los sistemas operativos que desea activar, es posible que deba instalar una o varias de estas actualizaciones:
> - Las instalaciones de KMS en Windows 7 o Windows Server 2008 R2 se deben actualizar para admitir la activación de clientes que ejecuten Windows 10. Para obtener más información, consulta  [Actualización que permite a los hosts de KMS de Windows 7 y Windows Server 2008 R2 KMS activar Windows 10](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10). 
> - Las instalaciones de KMS en Windows Server 2012, se deben actualizar para admitir la activación de clientes que ejecuten Windows 10 y Windows Server 2016 o sistemas operativos de cliente o servidor más recientes. Para obtener más información, consulta  [Paquete acumulativo de actualizaciones de julio de 2016 para Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012).
> - Las instalaciones de KMS en Windows 8.1 o Windows Server 2012 R2 se deben actualizar para admitir la activación de clientes que ejecuten Windows 10 y Windows Server 2016 o sistemas operativos de cliente o servidor más recientes. Para obtener más información, consulta  [Paquete acumulativo de actualizaciones de julio de 2016 para Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2). 
> - No se puede actualizar Windows Server 2008 R2 para que admita la activación de clientes que ejecutan Windows Server 2016 o sistemas operativos más recientes.

Un único host de KMS puede admitir una cantidad ilimitada de clientes de KMS. Si tienes más de 50 clientes, recomendamos que poseas al menos dos hosts de KMS, por si uno deja de estar disponible. La mayoría de las organizaciones puede funcionar con solo dos hosts de KMS para toda la infraestructura.

## <a name="addressing-kms-operational-requirements"></a>Abordar los requisitos operativos de KMS
KMS puede activar equipos físicos y virtuales, pero para calificar para la activación de KMS, una red debe tener un número mínimo de equipos (denominado umbral de activaciones). Los clientes de KMS se activan solo cuando se cumple este umbral. Para asegurarse de que se cumpla el umbral de activaciones, un host de KMS cuenta el número de equipos que está solicitando activación en la red.

Los hosts de KMS cuentan las conexiones más recientes. Cuando un cliente o servidor se pone en contacto con el host de KMS, el host agrega el identificador del equipo a su cuenta y, después, devuelve el valor de recuento actual en su respuesta. Si el recuento es bastante alto, se activará el cliente o servidor. Los clientes se activarán si el recuento es de 25 o superior. Los servidores y las ediciones de volumen de productos de Microsoft Office se activarán si el recuento es de 5 o superior. KMS solo cuenta conexiones únicas de los últimos 30 días y solo almacena los 50 contactos más recientes.

Las activaciones de KMS son válidas durante 180 días, un período se conoce como el intervalo de validez de activación. Los clientes de KMS deben renovar sus activaciones al conectarse al host de KMS al menos una vez cada 180 días para permanecer activados. De forma predeterminada, los equipos cliente de KMS intentan renovar sus activaciones cada siete días. Una vez que se renueva la activación de un cliente, el intervalo de validez de la activación vuelve a comenzar.

## <a name="addressing-kms-functional-requirements"></a>Requisitos funcionales de KMS

La activación de KMS requiere conectividad TCP/IP. Los clientes y los hosts de KMS están configurados de forma predeterminada para usar el Sistema de nombres de dominio (DNS). De manera predeterminada, los hosts de KMS usan la actualización dinámica de DNS para publicar de forma automática la información que los clientes de KMS necesitan para encontrarlos y conectarse a ellos. Puede aceptar esta configuración predeterminada o, si tiene requisitos de configuración de seguridad y de red especiales, puede configurar manualmente los clientes y los hosts de KMS.

Una vez que se activa el primer host de KMS, la clave de KMS que se usa en el primer host se puede usar para activar hasta cinco hosts de KMS más en su red. Una vez que se activa un host de KMS, los administradores pueden reactivar el mismo host hasta nueve veces con la misma clave.

Si la organización necesita más de seis hosts de KMS, debes solicitar activaciones adicionales para la clave de KMS de la organización; por ejemplo, si tienes diez ubicaciones físicas bajo un contrato de licencias por volumen y quieres que cada ubicación tenga un host de KMS local.

> [!NOTE]
> Para solicitar esta excepción, póngase en contacto con el Centro de llamadas de activación. Para obtener más información, vea [Licencias por volumen de Microsoft]( https://www.microsoft.com/licensing).

Los equipos que ejecutan las ediciones de licencias por volumen de Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 7 y Windows Server 2008 R2 son, de manera predeterminada, clientes de KMS que no necesitan configuraciones adicionales.

Si convierte un equipo de un host de KMS, MAK o edición comercial de Windows a un cliente de KMS, instale la clave de configuración del cliente de KMS correspondiente. Para más información, consulte  [KMS Client Setup Keys](KMSclientkeys.md) (Claves de configuración de cliente de KMS).
