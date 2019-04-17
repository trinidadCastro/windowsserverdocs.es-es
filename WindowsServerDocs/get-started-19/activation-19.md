---
title: Activación de Windows Server 2019
description: Cómo activar Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 0e7a61fe34965ecf47505e6825a5c03680b1b40d
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150875"
---
# Activación de Windows Server 2019

>Se aplica a: Windows Server 2019, Windows Server 2016

La siguiente información describe las consideraciones de planeación iniciales que debes revisar para la activación de servicios de administración de claves (KMS) que implican Windows Server 2019. Para obtener información sobre la activación de KMS en relación con los sistemas operativos anteriores a los que se indican aquí, consulta [Paso 1: Revisar y seleccionar métodos de activación](https://technet.microsoft.com/library/jj134256(WS.11).aspx).

KMS usa un modelo de cliente-servidor para activar los clientes. Los clientes de KMS se conectan a un servidor de KMS, denominado host de KMS, para la activación. El host de KMS debe residir en tu red local.

No es necesario que los hosts de KMS sean servidores dedicados. KMS puede hospedarse con otros servicios. Puedes ejecutar un host de KMS en cualquier sistema físico o virtual que se está ejecutando Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows 8.1 o Windows Server 2012.

Un host de KMS que se ejecuta en Windows10 o Windows8.1 solo puede activar equipos que ejecutan sistemas operativos cliente.
En la siguiente tabla se resume los requisitos de host y el cliente KMS para las redes que incluyen a los clientes de Windows Server 2016, Windows Server 2019 y Windows 10.

>[!NOTE]
>- Es posible que requieren actualizaciones en el servidor KMS para admitir la activación de cualquiera de estos clientes más recientes. Si recibes errores de activación, comprueba que cuentas con las actualizaciones correspondientes que aparecen a continuación de esta tabla.
>- Si estás trabajando con máquinas virtuales, consulte [La activación automática de máquina Virtual](vm-activation-19.md) para obtener información y las claves AVMA.

|Grupo de claves de producto|KMS puede estar hospedado en|Ediciones de Windows activadas por este host de KMS|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|Licencia por volumen para Windows Server 2019|Windows Server2012R2<br /><br />WindowsServer2016<br /><br />Windows Server 2019<br /><br />|Canal semianual de WindowsServer<br /><br />Windows Server 2019 (todas las ediciones)<br /><br />Windows Server 2016 (todas las ediciones)<br /><br />Windows 10 Enterprise LTSC 2019 <br /><br />Windows 10 Enterprise LTSC N 2019<br /><br />Windows 10 LTSB (2015 y 2016)<br /><br />Windows 10 Professional<br /><br />Windows 10 Enterprise<br /><br />Windows 10 Pro for Workstations<br /><br />Windows10Education<br /><br />Windows Server 2012 R2 (todas las ediciones)<br /><br />Windows 8.1 Professional<br /><br />Windows 8.1 Enterprise<br /><br />Windows Server 2012 (todas las ediciones)<br /><br />Windows Server 2008 R2 (todas las ediciones)<br /><br />Windows Server 2008 (todas las ediciones)<br /><br />Windows 7 Professional<br /><br />Windows 7 Enterprise<br />| 
|Licencia por volumen para Windows Server 2016|Windows Server 2012<br /><br />Windows Server2012R2<br /><br />WindowsServer2016<br /><br />|Canal semianual de WindowsServer <br><br>Windows Server 2016 (todas las ediciones)<br /><br />Windows 10 LTSB (2015 y 2016)<br /><br />Windows 10 Professional<br /><br />Windows 10 Enterprise<br /><br />Windows 10 Pro for Workstations<br><br>Windows10Education<br><br>Windows Server 2012 R2 (todas las ediciones)<br /><br />Windows 8.1 Professional<br /><br />Windows 8.1 Enterprise<br /><br />Windows Server 2012 (todas las ediciones)<br /><br />Windows Server 2008 R2 (todas las ediciones)<br /><br />Windows Server 2008 (todas las ediciones)<br /><br />Windows 7 Professional<br /><br />Windows 7 Enterprise<br /><br />| 
|Licencia por volumen para Windows 10|Windows7<br /><br /> Windows 8.1<br /><br /> Windows 10|Windows 10 Professional<br /><br /> Windows 10 Professional N<br /><br /> Windows10 Enterprise<br /><br /> Windows 10 Enterprise N<br /><br /> Windows10 Education<br /><br /> Windows 10 Education N<br /><br /> Windows 10 Enterprise LTSB (2015)<br /><br /> Windows 10 Enterprise LTSB N (2015)<br /><br /> Windows 10 Pro for Workstations<br><br>Windows 8.1 Professional<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows 7 Professional<br /><br /> Windows 7 Enterprise<br /><br />|  
|Licencia por volumen de "Windows Server 2012 R2 para Windows 10"|Windows Server2008R2<br /><br /> Windows Server 2012 Standard<br /><br /> Windows Server 2012 Datacenter<br /><br /> Windows Server 2012 R2 Standard<br /><br />Windows Server 2012 R2 Datacenter|Windows 10 Professional<br /><br /> Windows 10 Enterprise<br /><br />Windows 10 Enterprise LTSB (2015)<br><br>Windows 10 Pro for Workstations<br><br>Windows10Education<br><br> Windows Server 2012 R2 (todas las ediciones)<br /><br /> Windows 8.1 Professional<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows Server 2012 (todas las ediciones)<br /><br /> Windows Server 2008 R2 (todas las ediciones)<br /><br /> Windows Server 2008 (todas las ediciones)<br /><br />Windows 7 Professional<br /><br /> Windows 7 Enterprise|

> [!NOTE]  
>En función del sistema operativo que ejecuta el servidor KMS y los sistemas operativos que quieres activar, es posible que debas instalar una o varias de estas actualizaciones:
>- Las instalaciones de KMS en Windows 7 o Windows Server 2008 R2 se deben actualizar para admitir la activación de clientes que ejecuten Windows10. Para obtener más información, consulta la[actualización que permite a Windows 7 y los hosts de Windows Server 2008 R2 KMS activar Windows 10](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10).  
>- Las instalaciones de KMS en Windows Server 2012 se deben actualizar para admitir la activación de clientes que ejecuten Windows 10 y Windows Server 2016 o Windows Server 2019, o cliente más reciente o sistemas operativos de servidor. Para obtener más información, consulta[julio de 2016 acumulativo de actualizaciones para Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012). 
>- Las instalaciones de KMS en Windows 8.1 o Windows Server 2012 R2 se deben actualizar para admitir la activación de clientes que ejecuten Windows 10 y Windows Server 2016 o Windows Server 2019, o cliente más reciente o sistemas operativos de servidor. Para obtener más información, consulta[julio de 2016 acumulativo de actualizaciones para Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2).  
>- Windows Server 2008 R2 no se pueden actualizar para admitir la activación de clientes que ejecuten Windows Server 2016, Windows Server 2019 o sistemas operativos más recientes. 

Un único host de KMS puede admitir una cantidad ilimitada de clientes de KMS. Si tienes más de 50 clientes, recomendamos que tengas al menos dos hosts de KMS por si uno de los host de KMS deja de estar disponible. La mayoría de las organizaciones puede funcionar con solo dos hosts de KMS para toda la infraestructura.

# Abordar los requisitos operativos de KMS
KMS puede activar equipos físicos y virtuales, pero para calificar para la activación de KMS, una red debe tener un número mínimo de equipos (denominado "umbral de activaciones"). Los clientes de KMS se activan solo cuando se cumple este umbral. Para asegurarte de que se cumpla el umbral de activaciones, un host de KMS cuenta el número de equipos que solicita la activación en la red.

Los hosts de KMS cuentan las conexiones más recientes. Cuando un cliente o servidor se pone en contacto con el host de KMS, el host agrega el identificador del equipo a su cuenta y luego devuelve el valor de recuento actual en la respuesta. Si el recuento es bastante alto, se activará el cliente o servidor. Los clientes se activarán si el recuento es de 25 o superior. Los servidores y las ediciones de volumen de productos de Microsoft Office se activarán si el recuento es de 5 o superior. KMS solo cuenta conexiones únicas de los últimos 30 días y solo almacena los 50 contactos más recientes.

Las activaciones de KMS son válidas durante 180 días, un período que se conoce como "intervalo de validez de activación". Los clientes de KMS deben renovar sus activaciones al conectarse al host de KMS al menos una vez cada 180 días para permanecer activados. De manera predeterminada, los equipos cliente de KMS intentan renovar las activaciones cada siete días. Una vez que se renueva la activación de un cliente, el intervalo de validez de la activación vuelve a comenzar.

# Requisitos funcionales de KMS

La activación de KMS requiere conectividad TCP/IP. Los clientes y los hosts de KMS están configurados de manera predeterminada para usar el sistema de nombres de dominio (DNS). De manera predeterminada, los hosts de KMS usan la actualización dinámica de DNS para publicar de forma automática la información que los clientes de KMS necesitan para encontrarlos y conectarse a ellos. Puedes aceptar esta configuración predeterminada o, si tienes requisitos de configuración de seguridad y de red especiales, puedes configurar manualmente los clientes y los hosts de KMS.

Una vez que se activa el primer host de KMS, puedes usar la clave de KMS que se usa en el primer host para activar hasta cinco hosts de KMS más en tu red. Una vez que se activa un host de KMS, los administradores pueden reactivar el mismo host hasta nueve veces con la misma clave.

Si la organización necesita más de seis hosts de KMS, debe solicitar activaciones adicionales para la clave de KMS de la organización, por ejemplo, si tienes diez ubicaciones físicas bajo un contrato de licencias por volumen y quieres que cada ubicación tenga un host de KMS local.

>[!NOTE] 
>Para solicitar esta excepción, ponte en contacto con el Centro de llamadas de activación. Para obtener más información, consulta [Licencias por volumen de Microsoft](https://go.microsoft.com/fwlink/?LinkID=73076).

Los equipos que ejecutan las ediciones de licencia de volumen de Windows 10, Windows Server 2019, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 7, Windows Server 2008 R2 son, de manera predeterminada, los clientes de KMS con ninguna configuración adicional necesarios.

Si conviertes un equipo de un host de KMS, MAK o edición comercial de Windows a un cliente de KMS, instala la clave de configuración del cliente de KMS correspondiente. Para obtener más información, consulta[Claves de configuración de cliente de KMS](../get-started/KMSclientkeys.md). 
 

 
