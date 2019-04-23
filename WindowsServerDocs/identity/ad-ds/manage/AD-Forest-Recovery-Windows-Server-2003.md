---
title: Recuperación de bosques de AD - recuperación de Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: bd15df5360a50e417881d83319344dbdf48f35fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829646"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Recuperación de bosques de AD - recuperación de Windows Server 2003

>Se aplica a: Windows Server 2003

Este tema incluyen procedimientos de recuperación de bosques para controladores de dominio (DC) que ejecutan Windows Server 2003. El proceso general para la recuperación del bosque no es diferente con controladores de dominio de Windows Server 2003, pero los procedimientos específicos pueden diferir debido a las herramientas diferentes. Por ejemplo, se puede usar Ntdsutil.exe copia de seguridad y restauración de controladores de dominio que ejecutan controladores de dominio de Windows Server 2003, mientras que la copia de seguridad de Windows Server o Wbadmin.exe se usa para los controladores de dominio que ejecutan Windows Server 2008 o posterior.  
  
- [Copia los datos de estado del sistema](#Backing-up-the-System-State-data)  
- [Realizar una restauración no autoritativa](#Performing-a-nonauthoritative restore)  
- [Instalar y configurar el servicio servidor DNS](#Install-and-configure-the-DNS-Server-service)  

## <a name="backing-up-the-system-state-data"></a>Copia los datos de estado del sistema
Use el procedimiento siguiente para realizar una copia de seguridad de los datos de estado del sistema, junto con cualquier otro dato que seleccionó para la operación de copia de seguridad actual, de un controlador de dominio que ejecuta Windows Server 2003. Windows Server 2003 incluye la herramienta Ntbackup, que puede usar para realizar una copia de seguridad de los datos de estado del sistema.  
  
Pertenencia a **administradores** o **operadores de copia de seguridad**, o equivalente, es lo mínimo necesario para realizar una copia de seguridad de archivos y carpetas.   
  
Si copia los datos de estado del sistema de seguridad en una cinta y el programa de copia de seguridad indica que no hay ningún medio sin usar disponible, es posible que deba usar almacenamiento extraíble. Esto agrega la cinta para el grupo de medios libres para que esa copia de seguridad pueda usarlo.  
  
Solo puede realizar la copia de seguridad de los datos de estado del sistema en un equipo local. No se puede realizar backup en un equipo remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Para realizar una copia de seguridad de los datos de estado del sistema en un controlador de dominio que ejecuta Windows Server 2003  
  
1. Haga clic en **iniciar**, apunte a **todos los programas**, apunte a **Accesorios**, apunte a **herramientas del sistema**y, a continuación, haga clic en **copia de seguridad** .  
2. En el **bienvenida** página, haga clic en **modo avanzado**.  
3. En el **copia de seguridad** ficha, active la casilla de verificación para cualquier unidad, carpeta o archivo que desea realizar copias de seguridad.  
4. Seleccione el **del estado del sistema** casilla de verificación.  
5. Haga clic en **iniciar copia de seguridad**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Realizar una restauración no autoritativa  

Use el procedimiento siguiente para realizar una restauración no autoritativa de un controlador de dominio que ejecuta Windows Server 2003. Al realizar una restauración no autoritativa de Active Directory en Windows Server 2003, realizar automáticamente una restauración no autoritativa de SYSVOL. Se requiere ningún paso adicional.  
  
> [!NOTE]
> Si también va a reinstalar el sistema operativo Windows Server 2003, puede o no se puede unir el equipo al dominio y puede asignar cualquier nombre para el equipo durante la instalación del sistema operativo. No instale Active Directory. Después de reinstalar el sistema operativo, vaya directamente al paso 4.  
  
En los controladores de dominio de Windows Server 2003 donde haya restaurado los datos de estado del sistema, deberá volver a instalar las aplicaciones de software que se estaban ejecutando en los controladores de dominio antes de recuperación. Restauración de AD DS en el primer controlador de dominio en el dominio también restaura el registro, ya que ambos forman parte de los datos de estado del sistema. Téngalo en cuenta si tuviera las aplicaciones que se ejecutan en estos controladores de dominio y si tuvieran que toda la información almacenada en el registro.  
  
Para ahorrar tiempo requerido para volver a instalar el software, determinar si las aplicaciones que deben instalarse en los controladores de dominio son compatibles con la clonación de controlador de dominio virtual. Estas aplicaciones pueden instalarse en el DC de origen antes de la clonación con el fin de ahorrar el tiempo y esfuerzo requeridos para instalarlos en los controladores de dominio virtuales clonados.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Para realizar una restauración no autoritativa
  
1. Después de iniciar el controlador de dominio, presione F8 para reiniciar el equipo en el modo de restauración de servicios de directorio (DSRM).  
2. Seleccione **modo de restauración de servicios de directorio (solo controladores de dominio de Windows)**.  
3. Seleccione el sistema operativo que desea iniciar en modo de restauración.  
4. Inicie sesión como administrador (solo puede utilizar una cuenta de equipo local, ninguna opción de inicio de sesión de dominio está disponible).  
5. En un símbolo del sistema, escriba **ntbackup**, y, a continuación, presione ENTRAR.  
6. En el **bienvenida** página, haga clic en **modo avanzado**y, a continuación, seleccione el **restaurar y administrar medios** ficha. (No seleccione **Asistente para restauración**.)  
7. Seleccione el archivo de copia de seguridad adecuado para restaurar a partir de y asegúrese de que el **disco del sistema** y **del estado del sistema** casillas están activadas.  
8. Haga clic en **Iniciar restauración**.  
9. Una vez completada la operación de restauración, reinicie el equipo.  
  
Utilice el procedimiento siguiente para realizar una restauración autoritativa (también conocido como principal) de SYSVOL en un controlador de dominio que ejecuta Windows Server 2003. Realice este procedimiento sólo en el primer controlador de dominio de Windows Server 2003 a la que se restaura en el dominio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Para realizar una restauración autoritativa de SYSVOL  
  
1. Realice los pasos 1 a 8 del procedimiento anterior.  
2. En el **Confirmar restauración** cuadro de diálogo, haga clic en **avanzadas**.  
3. Para llevar a cabo una restauración autoritativa de SYSVOL, active la casilla de verificación **al restaurar los conjuntos de datos replicados, marcar los datos restaurados como los datos para todas las réplicas principales**.  

   > [!NOTE]
   > Marcar los datos restaurados como los datos principales en la copia de seguridad están equivalentes a establecer el **BurFlags** entrada a D4 bajo la subclave del registro siguiente:  
   >   
   > **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\\** *GUID*  

4. Una vez completada la operación de restauración, reinicie el equipo.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Instalar y configurar el servicio servidor DNS

Si el controlador de dominio que puede restaurar desde copia de seguridad se está ejecutando Windows Server 2003, puede instalar el servidor DNS sin conectar el controlador de dominio a ninguna red.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Para instalar y configurar el servicio servidor DNS  
  
1. Abra el Asistente para componentes de Windows. Para abrir al asistente:  

   - Haga clic en **Inicio**, haga clic en **Panel de Control** y, a continuación, haga clic en **Agregar o quitar programas**.  
   - Haga clic en **agregar o quitar componentes de Windows**.  

2. En **componentes**, seleccione el **servicios de red** casilla de verificación y, a continuación, haga clic en **detalles**.  
3. En **subcomponentes de servicios de red**, seleccione el **Domain Name System (DNS)** casilla de verificación, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
4. Si se le pide, en **copiar archivos desde**, escriba la ruta de acceso completa de los archivos de distribución y, a continuación, haga clic en **Aceptar**.  

   Después de la instalación, complete los pasos siguientes para configurar el servidor DNS.  

5. Haga clic en **iniciar**, apunte a **todos los programas**, apunte a **herramientas administrativas**y, a continuación, haga clic en **DNS**.  
6. Crear zonas DNS para los mismos nombres de dominio DNS que se hospedan en los servidores DNS antes de funcionamiento defectuoso crítico. Para obtener más información, vea Agregar una zona de búsqueda directa ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configurar los datos DNS tal como se encontraba antes el mal funcionamiento crítico. Por ejemplo:  

   - Configurar zonas DNS que se almacenan en AD DS. Para obtener más información, consulte Cambiar el tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configurar la zona DNS es autoritativa para registros de recursos del localizador (Ubicador de DC) del controlador de dominio permitir la actualización dinámica segura. Para obtener más información, consulte permitir sólo actualizaciones dinámicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Asegúrese de que la zona DNS primaria contiene registros de recursos de la delegación (nombre del servidor (NS) y pegar recursos de host (A) registros) para la zona secundaria que se hospeda en este servidor DNS. Para obtener más información, vea Crear una delegación de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Después de configurar DNS, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:  

   **net stop netlogon**

10. Escriba el siguiente comando y presione ENTRAR:  

   **Net start netlogon**

   > [!NOTE]
   > Inicio de sesión registrará los registros de recursos de ubicador de DC en DNS para este controlador de dominio. Si va a instalar el servicio servidor DNS en un servidor en el dominio secundario, este controlador de dominio no podrá registrar sus registros de inmediato. Esto es porque está aislado en la actualidad como parte del proceso de recuperación y su servidor DNS principal es el servidor DNS de raíz de bosque. Configurar este equipo con la misma dirección IP que tenía antes del desastre para evitar errores de búsqueda del servicio de controlador de dominio.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de bosques de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosques de AD - idear un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosques de AD: identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación de bosques de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosques de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosques de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosques de AD - recuperar un único dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosques de AD: recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
