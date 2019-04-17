---
title: "Recuperación de bosque de AD - servicio configurar servidor DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f24570965fd8b3f3e050779c42758865cbee2728
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Recuperación de bosque de AD - configurar el servicio de servidor DNS 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
Si no está instalado el rol de servidor DNS en el controlador de dominio que restaurar a partir de copia de seguridad, debes instalar y configurar el servidor DNS.  
  

## <a name="install-and-configure-the-dns-server-service"></a>Instalar y configurar el servicio de servidor DNS  
Para completar este paso para cada controlador de dominio restaurado que no se ejecuta como un servidor DNS, una vez completada la restauración.  
  
> [!NOTE]
>  Si el controlador de dominio que restaura desde la copia de seguridad ejecuta Windows Server 2008 R2, debes conectar el controlador de dominio a una red aislada para poder instalar el servidor DNS. A continuación, cada uno de los servidores DNS restaurados conectarse a una red aislada, mutuamente compartida. Ejecute repadmin /replsum para comprobar que la replicación funciona entre los servidores DNS restaurados. Después de comprobar la replicación, puede conectar los controladores de dominio restaurados a la red de producción si ya está instalado el rol de servidor DNS, puedes aplicar una revisión que permite que un servidor DNS iniciar mientras el servidor no está conectado a una red. Debe incluir la revisión en la imagen de instalación del sistema operativo durante los procesos de compilación automatizada. Para obtener más información acerca de la revisión, consulta [artículo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691). 

Completa los pasos de instalación y configuración a continuación.
  
### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Instalar y el servicio de servidor DNS mediante el administrador del servidor  
  
1.  Abre el administrador del servidor y haz clic en **agregar roles y características**.  
2.  En el Asistente para agregar funciones, si la **antes de comenzar** aparece en la página, haz clic en **siguiente**.  
3.  En la **tipo de instalación** pantalla selecciona **basada en rol o instalación basada en la característica** y haz clic en **siguiente**.
4.  En la **selección de servidor** seleccionar el servidor de pantalla y haz clic en **siguiente**.
5.  En la **Roles de servidor** pantalla selecciona **servidor DNS**, si se te pide clic **agregar características** y haz clic en **siguiente**.
6.  En la **características** pantalla clic **siguiente**.
7.  Consulta la información de la **servidor DNS** página y, a continuación, haz clic en **siguiente**.
![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8.  En la **confirmación** página, comprueba que se instalará el rol de servidor DNS y, a continuación, haz clic en **instalar**.  
  
     
### <a name="to-configure-the-dns-server-service"></a>Para configurar el servicio de servidor DNS 
1.  Abre el administrador del servidor, haz clic en **herramientas** y haz clic en **DNS**.
![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)    
2.  Crear las zonas DNS para los mismos nombres de dominio DNS que se hospedan en los servidores DNS antes de la crítica error de funcionamiento. Para obtener más información, vea Agregar una zona de búsqueda hacia delante ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
3.  Configurar los datos DNS, tal y como era antes de la crítica error de funcionamiento. Por ejemplo:  
  
    -   Configurar las zonas DNS almacenarse en AD DS. Para obtener más información, vea Cambiar el tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configurar la zona DNS que está autorizada para los registros de recursos de ubicador DC de controlador de dominio permitir que la actualización dinámica segura. Para obtener más información, consulta Permitir solo seguro actualizaciones dinámicas ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
4. Asegúrese de que la zona DNS principal contiene registros de recursos de la delegación (nombre del servidor (NS) y pegar recursos de host (A) registros) para la zona secundaria que está hospedado en este servidor DNS. Para obtener más información, consulta crear una delegación de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
5. Después de configurar DNS, puede acelerar el proceso de registro de los registros de NETLOGON.  
  
    > [!NOTE]
    >  Las actualizaciones dinámicas seguras solo funcionan cuando hay disponible un servidor de catálogo global.  
  
     En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
     **Net stop netlogon**  
  
6. Escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
     **Comando Net start netlogon**  

![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
