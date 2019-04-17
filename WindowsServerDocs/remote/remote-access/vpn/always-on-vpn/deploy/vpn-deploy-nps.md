---
title: Instalar y configurar el servidor NPS
description: Procesamiento del servidor NPS de solicitudes de conexión que se envían por el servidor VPN comprueba que el usuario tiene permiso para conectar la identidad del usuario y registra los aspectos de la solicitud de conexión que elegiste al configurar cuentas RADIUS en NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 9c142ad4d4321baf328707f29fa104247ff5149a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035771"
---
# Paso 4. Instalar y configurar el servidor de directivas de redes (NPS)

>   Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Siguiente:** paso 3. Configurar el servidor de acceso remoto para Always On VPN](vpn-deploy-ras.md)<br>
& #187;  [ **Siguiente:** paso 5. Configurar DNS y la configuración de Firewall](vpn-deploy-dns-firewall.md)


En este paso, se instala el servidor de directivas de redes (NPS) para el procesamiento de solicitudes de conexión que se envían por el servidor VPN:

- Realizar la autorización para comprobar que el usuario tiene permiso para conectarse.
- Realizar la autenticación para comprobar la identidad del usuario.
- La realización de cuentas para registrar los aspectos de la solicitud de conexión que elegiste al configurar cuentas RADIUS en NPS.

Los pasos descritos en esta sección te permiten realizar los siguientes elementos:

1.  En el equipo o máquina virtual que prevista para el servidor NPS e instalado en tu organización o la red corporativa, puedes instalar NPS.

   >[!TIP] 
   >Si ya tienes uno o varios servidores NPS en la red, no es necesario realizar la instalación de servidor NPS: en su lugar, puedes usar este tema para actualizar la configuración de un servidor NPS existente.

>[!NOTE]  
No puedes instalar el servicio de servidor de directivas de red en Windows Server Core.

2.  En el servidor NPS o corporativa de la organización, puedes configurar NPS para actuar como servidor RADIUS que procesa las solicitudes de conexión recibidas desde el servidor VPN.

## Instalar el Servidor de directivas de redes

En este procedimiento, puedes instalar NPS mediante Windows PowerShell o el administrador agregar Roles de servidor y Features Wizard. NPS es un servicio de rol del rol de servidor de servicios de acceso y directivas de red.

>[!TIP] 
>De manera predeterminada, NPS escucha el tráfico RADIUS en puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Cuando se instala NPS y habilitar Firewall de Windows con seguridad avanzada, excepciones de firewall para estos puertos se crean automáticamente para el tráfico de IPv4 e IPv6. Si los servidores de acceso de red están configurados para enviar tráfico RADIUS a través de los puertos que no sean de estos valores predeterminados, quitar las excepciones que se creó en Firewall de Windows con seguridad avanzada durante la instalación de NPS y crear excepciones para los puertos que usas para Tráfico RADIUS.

**Procedimiento para Windows PowerShell:**

Para realizar este procedimiento mediante Windows PowerShell, ejecuta Windows PowerShell como administrador, escribe el siguiente comando y, a continuación, presiona ENTRAR.

`Install-WindowsFeature NPAS -IncludeManagementTools`

**Procedimiento para el administrador del servidor:**

1.  En el Administrador de servidores, haga clic en **Administrar**y haz clic en **Agregar Roles y características**. <p>Abre el agregar Roles and Features Wizard.

2.  Antes de comenzar, haga clic en **siguiente**.

    >[!NOTE] 
    >No se muestra la página **Antes de comenzar** del Asistente de características y de agregar Roles si hubieras seleccionado previamente **Omitir esta página de manera predeterminada** cuando se ha ejecutado el agregar Roles y características de asistente.

1.  En Seleccionar el tipo de instalación, asegúrate de que esté seleccionado **instalación basada en características o basado en roles** y haga clic en **siguiente**.

2.  En Seleccionar servidor de destino, asegúrate de que **Seleccionar un servidor de grupo de servidores** está activada.

3.  En el grupo de servidores, asegúrese de que el equipo local está seleccionado y haz clic en **siguiente**.

4.  En Seleccionar Roles de servidor, en **Roles**, selecciona **la directiva de red y acceso a los servicios**. Un cuadro de diálogo abre que pregunta si deben agregar características necesarias para la directiva de red y acceso a los servicios.

5.  Haz clic en **Agregar características**y haz clic en **siguiente**

6.  Características de selección, haz clic en **siguiente**, en la directiva de red y servicios de acceso, revisa la información proporcionada y haga clic en **siguiente**.

7.  En los servicios de rol de selección, haz clic en **El servidor de directivas de red**.

8.  Para las características necesarias para el servidor de directivas de red, haz clic en **Agregar características** y haz clic en **siguiente**.

9.  Confirmar selecciones de instalación, haga clic en **reiniciar el servidor de destino automáticamente si es necesario**.

10. Haz clic en **Sí** para confirmar el seleccionado y, a continuación, haz clic en **instalar**. <p>La página de progreso de la instalación, muestra el estado durante el proceso de instalación. Cuando finalice el proceso, se muestra el mensaje "instalación se realizó correctamente en *nombreDeEquipo"*, donde *ComputerName* es el nombre del equipo en el que ha instalado el servidor de directivas de red.

11. Haz clic en **Cerrar**.

## Configurar NPS

Después de instalar NPS, configuración NPS para controlar toda la autenticación, la autorización, y los derechos de administración de cuentas de conexión solicitan recibe desde el servidor VPN.

### Registrar el servidor NPS en Active Directory

En este procedimiento, se registra el servidor en Active Directory para que tiene permiso para acceder a información de cuenta de usuario durante el procesamiento de solicitudes de conexión.

**Procedimiento:**

1.  En el Administrador de servidores, haz clic en **las herramientas**y, a continuación, haz clic en **El servidor de directivas de red**. Abre la consola NPS.

2.  En la consola NPS, haz clic en **NPS (Local)** y haz clic en **Registrar servidor en Active Directory** para seleccionarlo.<p>Se abrirá el cuadro de diálogo de servidor de directivas de red.

3.  En el cuadro de diálogo del servidor de directivas de red, haz clic en **Aceptar** dos veces.

Para obtener métodos alternativos del registro de NPS, consulte [registrar un servidor NPS en un dominio de Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### Configurar las cuentas de servidor de directivas de redes

En este procedimiento, configura la red la directiva de servidor Contabilidad mediante uno de los siguientes tipos de registro:

-   **El registro de eventos**. Se usa principalmente para la auditoría y solución de problemas de intentos de conexión. Puedes configurar registro mediante la obtención de las propiedades del servidor NPS en la consola de NPS de eventos NPS.

-   **Registro de autenticación de usuario y las solicitudes de cuentas en un archivo local**. Se usa principalmente para fines de análisis y la facturación de la conexión. También se usa como una herramienta de investigación de seguridad porque proporciona un método de seguimiento de la actividad de un usuario malintencionado después de un ataque. Puede configurar el registro de archivos local mediante el Asistente para configurar cuentas.

-   **Registro de autenticación de usuario y las solicitudes de cuentas a una base de datos compatible con XML de Microsoft SQL Server**. Usado para permitir que a varios servidores que ejecuten NPS tengan un origen de datos. También proporciona las ventajas de usar una base de datos relacional. Puede configurar el registro de SQL Server mediante el Asistente para la configuración de cuentas.

Para configurar cuentas de servidor de directiva de red, consulta la [Explicación de configurar la red la directiva de servidor](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### Agregue el servidor VPN como un cliente RADIUS

En la sección de [Configurar el servidor de acceso remoto para VPN de Always On](vpn-deploy-ras.md) , había instalado y había configurado el servidor VPN. Durante la configuración del servidor VPN, has agregado un secreto compartido en el servidor VPN. 

En este procedimiento, se usa la misma cadena de texto secreto compartido para configurar el servidor VPN como un cliente RADIUS en NPS. Usar la misma cadena de texto que usaste en el servidor VPN o se produce un error en la comunicación entre el servidor NPS y el servidor VPN.

>[!IMPORTANT] 
>Cuando agregas un servidor de acceso de red (servidor VPN, punto de acceso inalámbrico, conmutador de autenticación o servidor telefónico) a la red, debes agregar el servidor como un cliente RADIUS en NPS para que NPS es consciente de y puede comunicarse con el servidor de acceso de red.

**Procedimiento:**

1.  En el servidor NPS en la consola NPS, haz doble clic en **clientes y servidores RADIUS**.

2.  Haz clic en **Clientes RADIUS** y haga clic en **nuevo**. Abre el cuadro de diálogo nuevo cliente RADIUS.

3.  Comprueba que la casilla de verificación **Habilitar a este cliente RADIUS** está seleccionada.

4.  En el **nombre descriptivo**, escribe un nombre para mostrar para el servidor VPN.

5.  En la **dirección (IP o DNS)**, escribe la dirección IP de NAS o el FQDN.<p>Si escribes el FQDN, haz clic en **Comprobar** si quieres comprobar que el nombre es correcto y se asigna a una dirección IP.

6.  En **secreto compartido**, llevar a cabo:

    1.  Asegúrate de que se selecciona **Manual** .

    2.  Escribe la cadena de texto seguro que hayas especificado también en el servidor VPN.

    3.  Vuelva a escribir el secreto compartido en Confirmar secreto compartido.

7.  Haz clic en **Aceptar**. El servidor VPN aparece en la lista de clientes RADIUS configurado en el servidor NPS.

## Configurar NPS como un radio para las conexiones VPN

En este procedimiento, se configura NPS como servidor RADIUS en la red de la organización. En el NPS, debes definir una directiva que permite solo los usuarios en un grupo específico para tener acceso a la red de la organización o empresa a través del servidor VPN - y, después, únicamente cuando se usa un certificado de usuario válidas en una solicitud de autenticación de PEAP.

**Procedimiento:**

1.  En la consola NPS en una configuración estándar, asegúrate de que el **servidor RADIUS para conexiones VPN o telefónico** está activada.

2.  Haz clic en **Configurar VPN o acceso telefónico**.<p>Abre el Asistente para configurar VPN o telefónico.

3.  Haz clic en **conexiones de red privada Virtual (VPN)** y haz clic en **siguiente**.

4.  En especificar telefónico o servidor de VPN, en clientes RADIUS, selecciona el nombre del servidor VPN que hayas agregado en el paso anterior.<p>Por ejemplo, si el nombre de NetBIOS del servidor VPN es RAS1, haz clic en **RAS1**.

5.  Haz clic en **Siguiente**.

6.  En los métodos de autenticación de configurar, realiza los siguientes pasos:

    1.  Desactiva la casilla de verificación de **Autenticación cifrada de Microsoft versión 2 (MS-CHAPv2)** .

    2.  Haz clic en la casilla de verificación de **Protocolo de autenticación Extensible** para seleccionarlo.

    3.  En tipo (según el método de configuración de acceso y la red), haz clic en **Microsoft: EAP protegido (PEAP)** y haz clic en **Configurar**.<p>Abre el cuadro de diálogo de editar propiedades de EAP protegido.

    4.  Haz clic en **Eliminar** para quitar el tipo de EAP de contraseña segura (EAP-MSCHAP v2).

    5.  Haz clic en **Agregar**. Abre el cuadro de diálogo Agregar EAP.

    6.  Haz clic en la **tarjeta inteligente u otro certificado**y haz clic en **Aceptar**.

    7.  Haz clic en **Aceptar** para cerrar Editar propiedades de EAP protegido.

7.  Haz clic en **Siguiente**.

8.  Especificar grupos de usuario, realiza los siguientes pasos:

    1.  Haz clic en **Agregar**. Abre el cuadro de diálogo Seleccionar usuarios, equipos, cuentas de servicio o grupos.

    2.  Escribe **Los usuarios de VPN** y haz clic en **Aceptar**.

    3.  Haz clic en **Siguiente**.

9.  Especificar filtros de IP, haga clic en **siguiente**.

10. En especificar la configuración de cifrado, haz clic en **siguiente**. No se realiza los cambios.<p>Estas opciones de configuración se aplican solo a las conexiones de cifrado de punto a punto de Microsoft (MPPE), que no es compatible con este escenario.

11. Especifica un nombre de dominio Kerberos, haga clic en **siguiente**.

12. Haz clic en **Finalizar** para cerrar al asistente.

## Inscribir un certificado del servidor NPS

En este procedimiento, actualización la directiva de grupo en el servidor NPS local manualmente. Cuando las actualizaciones de la directiva de grupo, si se ha configurado la inscripción automática de certificados y funciona correctamente, el equipo local está inscriban automáticamente un certificado por la entidad de certificación (CA).

>[!NOTE]  
>La directiva de grupo se actualiza automáticamente cuando se reinicie el equipo de miembro de dominio o cuando un usuario inicia sesión en un equipo miembro del dominio. Además, la directiva de grupo se actualiza periódicamente. De manera predeterminada, esta actualización periódica se produce cada 90 minutos con un desplazamiento aleatorio de hasta 30 minutos.

La suscripción a **los administradores de TI**, o equivalente, es la cantidad mínima requerida para completar este procedimiento.

**Procedimiento:**

1.  En el NPS, abre Windows PowerShell.

2.  En el símbolo de Windows PowerShell, escriba **gpupdate**y, a continuación, presiona ENTRAR.

## Paso siguiente
[Paso 5. Configurar DNS y firewall de VPN de Always On](vpn-deploy-dns-firewall.md): en este paso, se instala servidor de directivas de redes (NPS) mediante Windows PowerShell o el administrador agregar Roles de servidor y Features Wizard. También configuración NPS para controlar todos los derechos de administración de cuentas de conexión, autorización y autenticación las solicitudes que recibe desde el servidor VPN.



---

