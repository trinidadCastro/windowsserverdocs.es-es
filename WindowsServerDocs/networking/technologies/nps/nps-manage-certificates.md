---
title: Administrar los certificados usados con NPS
description: Este tema proporciona información sobre el uso de certificados de servidor con el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2044cf30cc90c1673e05a1948ac9196d05940d1f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-used-with-nps"></a>Administrar los certificados usados con NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Si implementas un método de autenticación basada en certificados, como la seguridad de la capa de transporte de Protocol\ de autenticación Extensible \(EAP\-TLS\), \(PEAP\-TLS\) protegido Extensible seguridad de capa de transporte de Protocol\ de autenticación y la versión de Microsoft PEAP\ protocolo de autenticación 2 \ (v2\ MS\ CHAP), debe inscribir un certificado de servidor para todos los servidores NPS. El certificado de servidor debe:

- Cumplir con los requisitos de certificado de servidor mínimo como se describe en [configurar plantillas de certificado para PEAP y los requisitos de EAP](nps-manage-cert-requirements.md)

- Se ha emitido por una entidad de certificación \(CA\) que es de confianza para los equipos cliente. Una CA es de confianza cuando su certificado existe en el almacén de certificados de entidades de certificación raíz de confianza para el usuario actual y el equipo local.

Ayuden a las siguientes instrucciones de administración de certificados de servidor NPS en las implementaciones donde la CA de raíz de confianza es una CA de terceros, como Verisign, o una CA es que ha implementado para la infraestructura de clave pública \(PKI\) mediante el uso de servicios de certificados de Active Directory \(AD CS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Cambiar la expiración del identificador TLS almacenado en caché

Durante los procesos de autenticación inicial para EAP\ TLS, PEAP\-TLS y PEAP\-MS\-CHAPv2, el servidor NPS almacena en caché una parte de las propiedades de conexión de conexión del cliente TLS. El cliente también almacena en caché una parte de las propiedades de conexión del servidor NPS TLS.

Cada colección de estas propiedades de conexión TLS individual se denomina un identificador de TLS.

Los equipos cliente pueden almacenar en caché los identificadores TLS para varios autenticadores, mientras que los servidores NPS pueden almacenar en caché los identificadores TLS de muchos equipos cliente.

Los controladores de TLS almacenado en caché en el cliente y el servidor permiten el proceso de una nueva autenticación que se produzca más rápidamente. Por ejemplo, cuando un equipo inalámbrico vuelva a autenticar con un servidor NPS, el servidor NPS puede examinar el identificador de TLS para el cliente inalámbrico y puede determinar que la conexión de cliente es una reconexión rápidamente. El servidor NPS autoriza la conexión sin tener que realizar la autenticación completa.

En consecuencia, el cliente examina el identificador de TLS para el servidor NPS, determina que es una reconexión y no tiene que realizar la autenticación de servidor.

En equipos que ejecutan Windows 10 y Windows Server 2016, la expiración de identificador TLS predeterminado es 10 horas.

En algunos casos, es posible que quieras aumentar o disminuir el tiempo de caducidad de identificador TLS.

Por ejemplo, es posible que quieres reducir el tiempo de expiración TLS identificador en situaciones donde se revoca el certificado de un usuario por un administrador y el certificado se ha expirado. En este escenario, el usuario aún puede conectarse a la red si un servidor NPS tiene un identificador de TLS almacenado en caché que no haya caducado. Reducir la TLS expiración identificador es posible que ayuda a evitar estos usuarios con certificados ha revocado volviendo a conectar.

>[!NOTE]
>La mejor solución para este escenario es para deshabilitar la cuenta de usuario en Active Directory, o para quitar la cuenta de usuario del grupo de Active Directory que se concede permiso para conectarse a la red en la directiva de red. La propagación de estos cambios en todos los controladores de dominio es posible que también se retrasó, sin embargo, debido a la latencia de replicación. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurar la hora de expiración de identificador TLS en equipos cliente

Puedes usar este procedimiento para cambiar la cantidad de tiempo que los equipos cliente en caché el identificador de TLS de un servidor NPS. Después de autenticar correctamente un servidor NPS, los equipos cliente almacena en caché las propiedades de conexión de TLS del servidor NPS como un identificador de TLS. El identificador de TLS tiene una duración predeterminada de 10 horas \(36,000,000 milliseconds\). Puede aumentar o disminuir el tiempo de caducidad de identificador TLS mediante el siguiente procedimiento.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

>[!IMPORTANT]
>Este procedimiento se debe realizar en un servidor NPS, no en un equipo cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Para configurar la TLS controlar la hora de expiración en los equipos cliente

1. En un servidor NPS, abre el Editor del registro.

2. Busca la clave del registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. En la **editar** menú, haz clic en **nueva**y, a continuación, haz clic en **clave**.

4. Tipo **ClientCacheTime**, y, a continuación, presione ENTRAR.

5. Haz clic en **ClientCacheTime**, haz clic en **nueva**y, a continuación, haz clic en **valor DWORD (32 bits)**.

6. Escribe la cantidad de tiempo, en milisegundos, que quieres que los equipos cliente en la memoria caché el identificador de TLS de un servidor NPS tras la autenticación correcta primer intento por el servidor NPS.

## <a name="configure-the-tls-handle-expiry-time-on-nps-servers"></a>Configurar la hora de expiración de identificador TLS en los servidores NPS

Usa este procedimiento para cambiar la cantidad de tiempo que los servidores NPS caché el identificador de TLS de los equipos cliente. Después de autenticar correctamente un cliente de acceso, servidores NPS caché propiedades de la conexión del equipo cliente TLS como un identificador de TLS. El identificador de TLS tiene una duración predeterminada de 10 horas \(36,000,000 milliseconds\). Puede aumentar o disminuir el tiempo de caducidad de identificador TLS mediante el siguiente procedimiento.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

>[!IMPORTANT]
>Este procedimiento se debe realizar en un servidor NPS, no en un equipo cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-nps-servers"></a>Para configurar la TLS controlar la hora de expiración en los servidores NPS

1. En un servidor NPS, abre el Editor del registro.

2. Busca la clave del registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. En la **editar** menú, haz clic en **nueva**y, a continuación, haz clic en **clave**.

4. Tipo **ServerCacheTime**, y, a continuación, presione ENTRAR.

5. Haz clic en **ServerCacheTime**, haz clic en **nueva**y, a continuación, haz clic en **valor DWORD (32 bits)**.

6. Escribe la cantidad de tiempo, en milisegundos, que quieres que los servidores NPS para almacenar en caché el identificador de TLS de un equipo cliente tras la autenticación correcta primer intento por el cliente.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obtener el SHA-1 Hash de un certificado de CA de raíz de confianza

Usa este procedimiento para obtener el algoritmo de Hash seguro (SHA-1) el hash de una entidad de certificación raíz de confianza (CA) de un certificado que esté instalado en el equipo local. En algunas circunstancias, como al implementar la directiva de grupo, es necesario designar un certificado mediante el hash SHA-1 del certificado.

Al usar la directiva de grupo, puede designar uno o más certificados de entidad emisora de certificados raíz de confianza que los clientes deben usar para autenticar el servidor NPS durante el proceso de autenticación mutua con EAP o PEAP. Para designar un certificado de CA de raíz de confianza que los clientes deben usar para validar el certificado de servidor, puedes escribir el hash SHA-1 del certificado.

Este procedimiento muestra cómo obtener el hash SHA-1 de un certificado de CA de raíz de confianza mediante el complemento de certificados de Microsoft Management Console (MMC). 

Para completar este procedimiento, debe ser miembro de la **usuarios** grupo en el equipo local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Para obtener la SHA-1 hash de un certificado de CA de raíz de confianza

1. En el cuadro de diálogo Ejecutar o Windows PowerShell, escribe **mmc**, y, a continuación, presione ENTRAR. Abre el \(MMC\) Microsoft Management Console. En la consola MMC, haga clic en **archivo**, a continuación, haz clic en **agregar o quitar Snap\in**. La **agregar o quitar complementos** abre el cuadro de diálogo.

2. En **agregar o quitar complementos**, en **complementos disponibles**, haz doble clic en **certificados**. Abre el Asistente de complemento de certificados. Haz clic en **cuenta de equipo**y, a continuación, haz clic en **siguiente**.

3. En **Seleccionar equipo**, asegúrate de que **equipo Local (el equipo está ejecutando esta consola)** está seleccionada, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.

4. En el panel izquierdo, haz doble clic en **certificados (equipo Local)**y, a continuación, haz doble clic en el **entidades de certificación raíz de confianza** carpeta.

5. La **certificados** carpeta es una subcarpeta de la **entidades de certificación raíz de confianza** carpeta. Haz clic en el **certificados** carpeta.

6. En el panel de detalles, busque el certificado de la CA de raíz de confianza. Haz doble clic en el certificado. La **certificado** abre el cuadro de diálogo.

7. En la **certificado** cuadro de diálogo, haz clic en el **detalles** pestaña.

8. En la lista de campos, desplázate a y selecciona **huella digital**.

9. En el panel inferior, se muestra la cadena hexadecimal que es el hash SHA-1 de su certificado. Este comando selecciona la SHA-1 hash y, a continuación, presiona el método abreviado de teclado de Windows para la copia \(CTRL\+C\) para copiar el hash en el Portapapeles de Windows.

10. Abrir la ubicación a las que quieras pegar el hash SHA-1, ubicar correctamente el cursor y, a continuación, presiona el método abreviado de teclado de Windows para pegar comando \(CTRL\+V\). 

Para obtener más información acerca de certificados y NPS, consulta [configurar plantillas de certificado para PEAP y los requisitos de EAP](nps-manage-cert-requirements.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
