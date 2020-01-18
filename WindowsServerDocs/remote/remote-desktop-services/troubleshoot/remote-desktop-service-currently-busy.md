---
title: Al conectarse, el usuario recibe el mensaje "Remote Desktop Service is currently busy" (El servicio Escritorio remoto está ocupado actualmente)
description: Solución del error "Remote Desktop Service is currently busy" (El servicio Escritorio remoto está ocupado actualmente) cuando los usuarios inician una conexión de Escritorio remoto.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 989591f1d312446b680d708b2be7bea9b26ab8f9
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265887"
---
# <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Al conectarse, el usuario recibe el mensaje "Remote Desktop Service is currently busy" (El servicio Escritorio remoto está ocupado actualmente)

Para determinar una respuesta adecuada para este problema, consulta lo siguiente:

- El servicio Servicios de Escritorio remoto deja de responder (por ejemplo, el cliente de Escritorio remoto parece que se"bloquea" en la pantalla de inicio de sesión).  
   - Si el servicio no responde, consulta [Problema de memoria del servidor RDSH](#rdsh-server-memory-issue).
   - Si parece que el cliente interactúa con el servicio de forma normal, vete al paso siguiente.
- Si uno o varios usuarios desconectan sus sesiones de Escritorio remoto, ¿se pueden volver a conectar?  
   - Si el servicio sigue denegando las conexiones, independientemente del número de usuarios que desconecten sus sesiones, consulta [Problema del cliente de escucha de Escritorio remoto](#rd-listener-issue).
   - Si el servicio vuelve a aceptar conexiones después de que varios usuarios hayan desconectado sus sesiones, [comprueba la directiva del límite de conexiones](#check-the-connection-limit-policy).

## <a name="rdsh-server-memory-issue"></a>Problema de memoria del servidor RDSH

Se ha encontrado una fuga de memoria en algunos servidores RDSH de Windows Server 2012 R2. Con el tiempo, estos servidores empiezan a rechazar tanto las conexiones a Escritorio remoto como los inicios de sesión de la consola local con mensajes como estos:

> La tarea que intentas realizar no se puede completar porque Servicios de Escritorio remoto está ocupado actualmente. Inténtalo de nuevo al cabo de un rato. Otros usuarios podrán iniciar sesión.

Los clientes de Escritorio remoto que intentan conectarse también dejan de responder.

Para solucionar este problema, reinicia el servidor RDSH.

Para resolver este problema, aplica KB 4093114, [10 de abril de 2018: KB4093114 (paquete acumulativo mensual](https://support.microsoft.com/help/4093114/)), a los servidores RDSH.

## <a name="rd-listener-issue"></a>Problema del cliente de escucha de Escritorio remoto

Se ha detectado un problema en algunos servidores RDSH que se han actualizado directamente desde Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016. Cuando un cliente de Escritorio remoto se conecta al servidor RDSH, este crea un cliente de escucha de Escritorio remoto para la sesión del usuario. Los servidores afectados mantienen un recuento de los agentes de escucha de escritorio remoto que aumenta a medida que se conectan usuarios, pero nunca disminuye.

Puedes solucionar este problema con los métodos siguientes:

  - Reinicia el servidor RDSH para restablecer el número de clientes de escucha de escritorio remoto.
  - Modifica la directiva de límite de conexiones, establécela en un valor muy grande. Para más información acerca de la administración de una directiva del límite de conexiones, consulta [Comprobación de la directiva del límite de conexiones](#check-the-connection-limit-policy).

Para resolver este problema, aplique las siguientes actualizaciones a los servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del sistema operativo 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)

## <a name="check-the-connection-limit-policy"></a>Comprobación de la directiva de límite de conexiones

Puede establecer el límite del número de conexiones simultáneas de Escritorio remotas a nivel de equipo individual o mediante la configuración de un objeto de directiva de grupo (GPO). De manera predeterminada, no se establece el límite.

Para comprobar la configuración actual e identificar todos los GPO existentes en el servidor RDSH, abra una ventana del símbolo del sistema como administrador y escriba el siguiente comando:
  
```cmd
gpresult /H c:\gpresult.html
```
   
Cuando este comando finalice, abre **gpresult.html**. En **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto \\Conexiones**, busca la directiva **Limitar el número de conexiones**.

  - Si el valor de esta directiva es **Deshabilitado**, la directiva de grupo no limita las conexiones RDP.
  - Si el valor de esta directiva es **Habilitado**, comprueba **GPO prevalente**. Si tienes que quitar o cambiar el límite de conexiones, edita este GPO.

Para forzar los cambios en la directiva, abra una ventana del símbolo del sistema en el equipo afectado y escriba el siguiente comando:
  
```cmd
gpupdate /force
```
  
