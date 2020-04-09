---
title: 'Recuperación de bosque de AD: configurar el servicio servidor DNS'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 144a45f2a835d9cca60b5be5aac7569809c45b7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824178"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Recuperación del bosque de AD: configurar el servicio servidor DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Si el rol de servidor DNS no está instalado en el controlador de dominio que se restaura a partir de una copia de seguridad, debe instalar y configurar el servidor DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Instalar y configurar el servicio servidor DNS

Complete este paso para cada controlador de dominio restaurado que no se ejecute como un servidor DNS una vez finalizada la restauración. 

> [!NOTE]
> Si el controlador de dominio restaurado a partir de una copia de seguridad ejecuta Windows Server 2008 R2, debe conectar el controlador de dominio a una red aislada para poder instalar el servidor DNS. A continuación, Conecte cada uno de los servidores DNS restaurados a una red aislada mutuamente compartida. Ejecute repadmin/replsum para comprobar que funciona la replicación entre los servidores DNS restaurados. Después de comprobar la replicación, puede conectar los DC restaurados a la red de producción si el rol del servidor DNS ya está instalado, puede aplicar una revisión que permita que un servidor DNS se inicie mientras el servidor no está conectado a ninguna red. Debe integrar la revisión en la imagen de instalación del sistema operativo durante los procesos de compilación automatizada. Para obtener más información acerca de la revisión, consulte el [artículo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) de Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691). 

Complete los pasos de instalación y configuración que se indican a continuación.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Para instalar y el servicio servidor DNS mediante Administrador del servidor  

1. Abra Administrador del servidor y haga clic en **Agregar roles y características**. 
2. En el Asistente para agregar roles, si aparece la página **Antes de comenzar**, haga clic en **Siguiente**. 
3. En la pantalla **tipo de instalación** , seleccione Instalación basada en **características o en roles** y haga clic en **siguiente**.
4. En la pantalla **selección de servidor** , seleccione el servidor y haga clic en **siguiente**.
5. En la pantalla **roles de servidor** , seleccione **servidor DNS**. Si se le pide, haga clic en **Agregar características** y en **siguiente**.
6. En la pantalla **características** , haga clic en **siguiente**.
7. Lea la información de la página **servidor DNS** y, a continuación, haga clic en **siguiente**.
   ![](media/AD-Forest-Recovery-Configure-DNS/dns1.png) del servidor DNS  
8. En la página **confirmación** , compruebe que se instalará el rol de servidor DNS y, a continuación, haga clic en **instalar**. 

### <a name="to-configure-the-dns-server-service"></a>Para configurar el servicio servidor DNS

1. Abra Administrador del servidor, haga clic en **herramientas** y en **DNS**.
   ![](media/AD-Forest-Recovery-Configure-DNS/dns2.png) del servidor DNS
2. Cree zonas DNS para los mismos nombres de dominio DNS que se hospedaron en los servidores DNS antes de que el funcionamiento sea crítico. Para obtener más información, consulte Agregar una zona de búsqueda directa ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configure los datos DNS tal como existían antes de que el funcionamiento sea crítico. Por ejemplo:  

   - Configure las zonas DNS que se van a almacenar en AD DS. Para obtener más información, consulte cambiar el tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configure la zona DNS que sea autoritativa para los registros de recursos del localizador de controladores de dominio (Ubicador de DC) para permitir la actualización dinámica segura. Para obtener más información, vea permitir solo actualizaciones dinámicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Asegúrese de que la zona DNS primaria contiene registros de recursos de delegación (registros de recursos de servidor de nombres (NS) y de host de adherencia (A) para la zona secundaria hospedada en este servidor DNS. Para obtener más información, consulte crear una delegación de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Después de configurar DNS, puede acelerar el registro de los registros de NETLOGON.

   > [!NOTE]
   > Las actualizaciones dinámicas seguras solo funcionan cuando hay disponible un servidor de catálogo global. 

   En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   **net stop Netlogon**  

6. Escriba el siguiente comando y presione ENTRAR:  

   **net start Netlogon**  

   ![DNS Server](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
