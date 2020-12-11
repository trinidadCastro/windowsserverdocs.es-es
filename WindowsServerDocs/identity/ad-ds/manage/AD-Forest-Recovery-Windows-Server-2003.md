---
description: 'Más información acerca de la recuperación de bosque de AD: recuperación de Windows Server 2003'
title: 'Recuperación de bosque de AD: recuperación de Windows Server 2003'
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 132d65427baec7f11855d96d7174861ba9c8736f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049543"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Recuperación de bosque de AD: recuperación de Windows Server 2003

>Se aplica a: Windows Server 2003

En este tema se incluyen los procedimientos de recuperación de bosques para controladores de dominio (DC) que ejecutan Windows Server 2003. El proceso general para la recuperación del bosque no es diferente con los controladores de DC de Windows Server 2003, pero los procedimientos específicos pueden diferir debido a las distintas herramientas. Por ejemplo, Ntdsutil.exe se puede usar para hacer una copia de seguridad y restaurar los controladores de seguridad que ejecutan Windows Server 2003 DC, mientras que se usa Copias de seguridad de Windows Server o Wbadmin.exe para los controladores de seguridad que ejecutan Windows Server 2008 o posterior.

- [Copia de seguridad de los datos de estado del sistema](#backing-up-the-system-state-data)
- [Realizar una restauración no autoritativa](#performing-a-nonauthoritative-restore)
- [Instalar y configurar el servicio servidor DNS](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>Copia de seguridad de los datos de estado del sistema
Use el procedimiento siguiente para realizar una copia de seguridad de los datos de estado del sistema, junto con cualquier otro dato que haya seleccionado para la operación de copia de seguridad actual, de un controlador de dominio que ejecute Windows Server 2003. Windows Server 2003 incluye la herramienta ntbackup, que puede usar para realizar copias de seguridad de los datos de estado del sistema.

La pertenencia a **administradores** o **operadores de copia de seguridad**, o equivalente, es lo mínimo necesario para realizar una copia de seguridad de archivos y carpetas.

Si va a realizar una copia de seguridad de los datos de estado del sistema en una cinta y el programa de copia de seguridad indica que no hay ningún medio sin usar disponible, es posible que tenga que usar almacenamiento extraíble. Esto agrega la cinta al grupo de medios libres para que la copia de seguridad pueda usarla.

Solo se pueden realizar copias de seguridad de los datos de estado del sistema en un equipo local. No se puede realizar una copia de seguridad en un equipo remoto.

### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Para realizar una copia de seguridad de los datos de estado del sistema en un controlador de dominio que ejecuta Windows Server 2003

1. Haga clic en **Inicio**, seleccione **todos los programas**, **accesorios**, **herramientas del sistema** y, a continuación, haga clic en **copia de seguridad**.
2. En la página de **bienvenida** , haga clic en **modo avanzado**.
3. En la pestaña **copia de seguridad** , active la casilla de cualquier unidad, carpeta o archivo del que desee realizar una copia de seguridad.
4. Active la casilla **Estado del sistema** .
5. Haga clic en **Iniciar copia de seguridad**.

## <a name="performing-a-nonauthoritative-restore"></a>Realizar una restauración no autoritativa

Use el procedimiento siguiente para realizar una restauración no autoritativa de un controlador de dominio que ejecute Windows Server 2003. Al realizar una restauración no autoritativa en Active Directory en Windows Server 2003, se realiza automáticamente una restauración no autoritativa de SYSVOL. No se necesita realizar ningún paso adicional.

> [!NOTE]
> Si también está reinstalando el sistema operativo Windows Server 2003, es posible que el equipo se una al dominio o no, y que se pueda asignar cualquier nombre al equipo durante la instalación del sistema operativo. No instale Active Directory. Después de volver a instalar el sistema operativo, vaya directamente al paso 4.

En los controladores de dominio de Windows Server 2003 en los que solo se han restaurado los datos de estado del sistema, debe volver a instalar las aplicaciones de software que se estaban ejecutando en los controladores de dominio antes de la recuperación. La restauración de AD DS en el primer DC del dominio también restaura el registro porque ambos forman parte de los datos de estado del sistema. Tenga esto en cuenta si tiene aplicaciones que se ejecutan en estos controladores de sistema y si tenían alguna información almacenada en el registro.

Para ahorrar tiempo necesario para volver a instalar el software, determine si las aplicaciones que deben instalarse en los controladores de dominio son compatibles con la clonación de DC virtual. Estas aplicaciones se pueden instalar en el controlador de dominio de origen antes de la clonación para ahorrar el tiempo y el esfuerzo necesarios para instalarlos en los controladores de dominio virtuales clonados.

### <a name="to-perform-a-nonauthoritative-restore"></a>Para realizar una restauración no autoritativa

1. Después de iniciar el controlador de dominio, presione F8 para reiniciar el equipo en Modo de restauración de servicios de directorio (DSRM).
2. Seleccione **modo de restauración de servicios de directorio (solo controladores de dominio de Windows)**.
3. Seleccione el sistema operativo que desea iniciar en modo de restauración.
4. Inicie sesión como administrador (solo puede usar una cuenta de equipo local, no hay disponible ninguna opción de inicio de sesión de dominio).
5. En un símbolo del sistema, escriba **NTBackup** y, a continuación, presione Entrar.
6. En la página de **bienvenida** , haga clic en **modo avanzado** y, a continuación, seleccione la pestaña **restaurar y administrar medios** . (no seleccione **Asistente para restauración**).
7. Seleccione el archivo de copia de seguridad que desea restaurar y asegúrese de que estén activadas las casillas **disco del sistema** y **Estado del sistema** .
8. Haga clic en **Iniciar restauración**.
9. Una vez completada la operación de restauración, reinicie el equipo.

Use el procedimiento siguiente para realizar una restauración autoritativa (también conocida como principal) de SYSVOL en un controlador de dominio que ejecute Windows Server 2003. Realice este procedimiento solo en el primer DC de Windows Server 2003 que se restaure en el dominio.

### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Para realizar una restauración autoritativa de SYSVOL

1. Realice los pasos del 1 al 8 del procedimiento anterior.
2. En el cuadro de diálogo **confirmar restauración** , haga clic en **Opciones avanzadas**.
3. Para realizar una restauración autoritativa de SYSVOL, active la casilla **al restaurar conjuntos de datos replicados, marque los datos restaurados como los datos principales de todas las réplicas**.

   > [!NOTE]
   > Marcar los datos restaurados como datos principales en la copia de seguridad equivale a establecer la entrada **BurFlags** en D4 en la subclave del Registro siguiente:
   >
   > **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\\** *GUID* de

4. Una vez completada la operación de restauración, reinicie el equipo.

## <a name="install-and-configure-the-dns-server-service"></a>Instalar y configurar el servicio servidor DNS

Si el controlador de dominio restaurado a partir de una copia de seguridad ejecuta Windows Server 2003, puede instalar el servidor DNS sin conectar el controlador de dominio a ninguna red.

### <a name="to-install-and-configure-the-dns-server-service"></a>Para instalar y configurar el servicio servidor DNS

1. Abra el Asistente para componentes de Windows. Para abrir el asistente:

   - Haga clic en **Inicio**, haga clic en **Panel de Control** y, a continuación, haga clic en **Agregar o quitar programas**.
   - Haga clic en **Agregar o quitar componentes de Windows**.

2. En **componentes**, active la casilla **servicios de red** y, a continuación, haga clic en **detalles**.
3. En **Subcomponentes de servicios de red**, active la casilla **sistema de nombres de dominio (DNS)** , haga clic en **Aceptar** y, a continuación, haga clic en **siguiente**.
4. Si se le solicita, en **copiar archivos de**, escriba la ruta de acceso completa de los archivos de distribución y, a continuación, haga clic en **Aceptar**.

   Después de la instalación, complete los pasos siguientes para configurar el servidor DNS.

5. Haga clic en **Inicio**, seleccione **todos los programas**, seleccione **herramientas administrativas** y, a continuación, haga clic en **DNS**.
6. Cree zonas DNS para los mismos nombres de dominio DNS que se hospedaron en los servidores DNS antes de que el funcionamiento sea crítico. Para obtener más información, consulte Agregar una zona de búsqueda directa ( [https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574) ).
7. Configure los datos DNS tal como existían antes de que el funcionamiento sea crítico. Por ejemplo:

   - Configure las zonas DNS que se van a almacenar en AD DS. Para obtener más información, consulte cambiar el tipo de zona ( [https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579) ).
   - Configure la zona DNS que sea autoritativa para los registros de recursos del localizador de controladores de dominio (Ubicador de DC) para permitir la actualización dinámica segura. Para obtener más información, vea permitir solo actualizaciones dinámicas seguras ( [https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580) ).

8. Asegúrese de que la zona DNS primaria contiene registros de recursos de delegación (registros de recursos de servidor de nombres (NS) y de host de adherencia (A) para la zona secundaria hospedada en este servidor DNS. Para obtener más información, consulte crear una delegación de zona ( [https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562) ).
9. Después de configurar DNS, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:

   **net stop netlogon**

10. Escriba el siguiente comando y presione ENTRAR:

    **net start netlogon**

    > [!NOTE]
    > Net Logon registrará los registros de recursos del Ubicador de DC en DNS para este DC. Si va a instalar el servicio servidor DNS en un servidor del dominio secundario, este DC no podrá registrar sus registros inmediatamente. Esto se debe a que actualmente está aislado como parte del proceso de recuperación y su servidor DNS principal es el servidor DNS raíz del bosque. Configure este equipo con la misma dirección IP que tenía antes del desastre para evitar errores de búsqueda de servicio de DC.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)
- [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)
- [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
- [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)
- [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
