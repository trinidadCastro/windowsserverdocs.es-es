---
title: Recuperación de bosques de AD - servicio de configuración de servidor DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2c37428a0fb685e6a7fa4875366f3cd13401bd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842966"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Recuperación de bosques de AD - configurar el servicio servidor DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Si el rol de servidor DNS no está instalado en el controlador de dominio que va a restaurar desde copia de seguridad, debe instalar y configurar el servidor DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Instalar y configurar el servicio servidor DNS

Complete este paso para cada controlador de dominio restaurado que no se ejecuta como un servidor DNS una vez completada la restauración. 

> [!NOTE]
> Si el controlador de dominio que puede restaurar desde copia de seguridad se está ejecutando Windows Server 2008 R2, debe conectar el controlador de dominio a una red aislada para instalar el servidor DNS. A continuación, conectar cada uno de los servidores DNS restaurados a una red aislada y mutuamente compartida. Ejecute repadmin/Replsum para comprobar que la replicación funciona entre los servidores DNS restaurados. Después de comprobar la replicación, los controladores de dominio restaurados puede conectarse a la red de producción si el rol de servidor DNS ya está instalado, puede aplicar una revisión que permite que un servidor DNS iniciar mientras el servidor no está conectado a ninguna red. Debe incluir el hotfix en imagen de instalación del sistema operativo durante los procesos de compilación automatizado. Para obtener más información acerca de la revisión, consulte [artículo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691). 

Complete los pasos de instalación y la configuración siguiente.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Instalación y el servicio servidor DNS mediante el administrador del servidor  

1. Abra el administrador del servidor y haga clic en **agregar roles y características**. 
2. En el Asistente para agregar roles, si aparece la página **Antes de comenzar**, haga clic en **Siguiente**. 
3. En el **tipo de instalación** pantalla select **basada en roles o instalación basada en características** y haga clic en **siguiente**.
4. En el **selección de servidor** pantalla Seleccione el servidor y haga clic en **siguiente**.
5. En el **Roles de servidor** pantalla select **servidor DNS**, si se le solicite clic **agregar características** y haga clic en **siguiente**.
6. En el **características** pantalla clic **siguiente**.
7. Lea la información de la **servidor DNS** página y, a continuación, haga clic en **siguiente**.
   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. En el **confirmación** página, compruebe que se instalará el rol de servidor DNS y, a continuación, haga clic en **instalar**. 

### <a name="to-configure-the-dns-server-service"></a>Para configurar el servicio servidor DNS

1. Abra el administrador del servidor, haga clic en **herramientas** y haga clic en **DNS**.
   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Crear zonas DNS para los mismos nombres de dominio DNS que se hospedan en los servidores DNS antes de funcionamiento defectuoso crítico. Para obtener más información, vea Agregar una zona de búsqueda directa ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configurar los datos DNS tal como se encontraba antes el mal funcionamiento crítico. Por ejemplo:  

   - Configurar zonas DNS que se almacenan en AD DS. Para obtener más información, consulte Cambiar el tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configurar la zona DNS es autoritativa para registros de recursos del localizador (Ubicador de DC) del controlador de dominio permitir la actualización dinámica segura. Para obtener más información, consulte permitir sólo actualizaciones dinámicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Asegúrese de que la zona DNS primaria contiene registros de recursos de la delegación (nombre del servidor (NS) y pegar recursos de host (A) registros) para la zona secundaria que se hospeda en este servidor DNS. Para obtener más información, vea Crear una delegación de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Después de configurar DNS, puede acelerar el proceso de registro de los registros de NETLOGON.

   > [!NOTE]
   > Las actualizaciones dinámicas seguras sólo funcionan cuando está disponible un servidor de catálogo global. 

   En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   **net stop netlogon**  

6. Escriba el siguiente comando y presione ENTRAR:  

   **Net start netlogon**  

   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
