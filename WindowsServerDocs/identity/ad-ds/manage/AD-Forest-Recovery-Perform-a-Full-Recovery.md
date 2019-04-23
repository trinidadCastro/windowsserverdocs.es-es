---
title: Recuperación de bosques de AD - realizar una recuperación completa del servidor
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 6f600ade3d07130d4e1fb3b1a254cb1073f592e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874236"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Recuperación de bosques de AD - realizar una recuperación completa del servidor 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Utilice el procedimiento siguiente para realizar una recuperación completa del servidor para Windows Server 2016, 2012 R2 o 2012. 

## <a name="active-directory-full-server-recovery"></a>Recuperación de servidor completo de Active Directory

Una recuperación completa del servidor es necesario si va a restaurar a un hardware diferente o una instancia de sistema operativo diferente. No olvides lo siguiente:

- El número de unidades en las necesidades de servidor de destino para que sea igual al número de la copia de seguridad y que necesitan para ser el mismo tamaño o mayor.
- El servidor de destino debe iniciarse desde el DVD del sistema operativo para tener acceso a la **reparar el equipo** opción. 
- Si el controlador de dominio se está ejecutando en una máquina virtual en Hyper-V y la copia de seguridad de destino se almacena en una ubicación de red, debe instalar a un adaptador de red heredado. 
- Después de realizar una recuperación completa del servidor, deberá realizar una restauración autoritativa de SYSVOL, de forma independiente como se describe en [recuperación de bosques de AD - realizar una sincronización autoritativa de SYSVOL DFSR replicado](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

Según el escenario, utilice uno de los siguientes procedimientos para realizar una restauración completa. 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Realizar una restauración completa del servidor con una copia de seguridad local con la imagen más reciente
  
1. Iniciar la instalación de Windows, especifique el idioma, hora y formato de moneda y las opciones de teclado y haga clic en **siguiente**. 
2. Haga clic en **Reparar el equipo**.
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. Haz clic en **Solucionar problemas**.</br>
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. Haga clic en **recuperación de imagen del sistema**.</br>
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. Haga clic en **Windows Server 2016**. 
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. Si va a restaurar la última copia de seguridad local, haga clic en **usar la imagen de sistema más reciente disponible (recomendado)** y haga clic en **siguiente**.
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. Ahora tendrá una opción para:
   -  Formatear y volver a particionar discos
   -  Instalar controladores
   -  Anulando la selección de la **avanzadas** características de reiniciar automáticamente y la comprobación de errores de disco. Están habilitados de manera predeterminada.
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Haz clic en **Siguiente**.
9. Haga clic en **Finalizar**. Se le pedirá que le pregunta si está seguro de que desea continuar. Haga clic en **Sí**. 
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Cuando se complete, realice una restauración autoritativa de SYSVOL, como se describe en [recuperación de bosques de AD - realizar una sincronización autoritativa de SYSVOL DFSR replicado](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Realizar una restauración completa del servidor con una imagen local o remota

1. Iniciar la instalación de Windows, especifique el idioma, hora y formato de moneda y las opciones de teclado y haga clic en **siguiente**. 
2. Haga clic en **Reparar el equipo**.</br>
3. Haga clic en **solucionar**, haga clic en **recuperación de imagen del sistema**y haga clic en **Windows Server 2016**. 
4. Si va a restaurar la última copia de seguridad local, haga clic en **seleccionar una imagen de sistema** y haga clic en **siguiente**.
5. Ahora puede seleccionar la ubicación de la copia de seguridad que desea restaurar. Si la imagen es local puede seleccionarla en la lista. 
6. Si la imagen está en un recurso compartido de red, seleccione **avanzadas**. También puede seleccionar **avanzadas** si necesita instalar un controlador.
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. Si va a restaurar desde la red después de hacer clic **avanzadas** seleccione **buscar una imagen de sistema en la red**. Se pedirá que restaure la conectividad de red. Seleccione Aceptar. </br>
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Escriba la ruta de acceso UNC a la ubicación del recurso compartido de copia de seguridad (por ejemplo, \\\server1\backups) y haga clic en **Aceptar**. También puede escribir la dirección IP del servidor de destino, como \\\192.168.1.3\backups. 
   ![Restauración del servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. Escriba las credenciales necesarias para acceder al recurso compartido y haga clic en Aceptar. 
11. Ahora **seleccione la fecha y hora de la imagen del sistema para restaurar** y haga clic en **siguiente**.
12. Ahora tendrá una opción para:
   - Formatear y volver a particionar discos
   - Instalar controladores
   - Anulando la selección de la **avanzadas** características de reiniciar automáticamente y la comprobación de errores de disco. Están habilitados de manera predeterminada.
13. Haz clic en **Siguiente**.
14. Haga clic en **Finalizar**. Se le pedirá que le pregunta si está seguro de que desea continuar. Haga clic en **Sí**.  
15. Cuando se complete, realice una restauración autoritativa de SYSVOL, como se describe en [recuperación de bosques de AD - realizar una sincronización autoritativa de SYSVOL DFSR replicado](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Habilitar al adaptador de red para una copia de seguridad de red

Si tiene que habilitar un adaptador de red desde el símbolo del sistema restaurar desde un recurso compartido de red, siga estos pasos.

1. Iniciar la instalación de Windows, especifique el idioma, hora y formato de moneda y las opciones de teclado y haga clic en **siguiente**. 
2. Haga clic en **Reparar el equipo**. I
3. Haga clic en **solucionar**, haga clic en **símbolo**. 
4. Escriba el siguiente comando y presione ENTRAR:  

   ```  
   wpeinit  
   ```

5. Para confirmar el nombre del adaptador de red, escriba:  

   ```  
   show interfaces  
   ```  

   Escriba los comandos siguientes y presione ENTRAR después de cada comando:  

   ```  
   netsh  
   ```  

   ```  
   interface  
   ```  
  
   ```  
   tcp  
   ```  

   ```  
   ipv4  
   ```  
  
   ```  
   set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
   ```  

   Por ejemplo:  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   Tipo `quit` para volver a un símbolo del sistema. Tipo `ipconfig /all` para comprobar la red adaptador tiene una dirección IP y vuelva a hacer ping a la dirección IP del servidor que hospeda el recurso compartido de copia de seguridad para confirmar la conectividad. Cuando haya terminado, cierre el símbolo del sistema. 

6. Ahora que funciona el adaptador de red, seleccione los pasos anteriores para completar la restauración.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
