---
title: 'Recuperación de bosque de AD: realización de una recuperación de servidor completa'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 1ade1f2e316387fbe84209c1bc7a986fff6f2a71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390536"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Recuperación de bosque de AD: realización de una recuperación de servidor completa 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para realizar una recuperación completa del servidor para Windows Server 2016, 2012 R2 o 2012. 

## <a name="active-directory-full-server-recovery"></a>Active Directory la recuperación completa del servidor

Si va a realizar la restauración en otro hardware o en otra instancia del sistema operativo, es necesario realizar una recuperación completa del servidor. No olvides lo siguiente:

- El número de unidades en el servidor de destino debe ser igual al número de la copia de seguridad y deben ser del mismo tamaño o mayor.
- El servidor de destino debe iniciarse desde el DVD del sistema operativo para tener acceso a la opción **reparar el equipo** . 
- Si el controlador de dominio de destino se está ejecutando en una máquina virtual en Hyper-V y la copia de seguridad se almacena en una ubicación de red, debe instalar un adaptador de red heredado. 
- Después de realizar una recuperación completa del servidor, debe realizar una restauración autoritativa de SYSVOL por separado, como se describe en [recuperación del bosque de ad, realizar una sincronización autoritativa de SYSVOL replicada en DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

En función de su escenario, use uno de los procedimientos siguientes para realizar una restauración completa. 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Realizar una restauración completa del servidor con una copia de seguridad local con la imagen más reciente
  
1. Inicie instalación de Windows, especifique el idioma, el formato de hora y moneda y las opciones de teclado y haga clic en **siguiente**. 
2. Haga clic en **Reparar el equipo**.
   ![Server restore @ no__t-1
3. Haz clic en **Solucionar problemas**.</br>
   ![Server restore @ no__t-1
4. Haga clic en **recuperación de imagen del sistema**.</br>
   ![Server restore @ no__t-1
5. Haga clic en **Windows Server 2016**. 
   ![Server restore @ no__t-1
6. Si va a restaurar la copia de seguridad local más reciente, haga clic en **usar la imagen de sistema más reciente disponible (recomendado)** y haga clic en **siguiente**.
   ![Server restore @ no__t-1
7. Ahora se le ofrecerá una opción para:
   -  Formatear y volver a particionar discos
   -  Instalación de controladores
   -  Anulación de la selección de las características **avanzadas** de reinicio automático y comprobación de errores de disco. Están habilitados de forma predeterminada.
   ![Server restore @ no__t-1
8. Haz clic en **Siguiente**.
9. Haga clic en **Finalizar**. Se le preguntará si está seguro de que desea continuar. Haga clic en **Sí**. 
   ![Server restore @ no__t-1 
10. Una vez completado este paso, realice una restauración autoritativa de SYSVOL, como se describe en [recuperación del bosque de ad: realización de una sincronización autoritativa de SYSVOL replicada en DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Realizar una restauración completa del servidor con cualquier imagen local o remota

1. Inicie instalación de Windows, especifique el idioma, el formato de hora y moneda y las opciones de teclado y haga clic en **siguiente**. 
2. Haga clic en **Reparar el equipo**.</br>
3. Haga clic en **solucionar problemas**, en **recuperación de imagen del sistema**y en **Windows Server 2016**. 
4. Si va a restaurar la copia de seguridad local más reciente, haga clic en **seleccionar una imagen del sistema** y haga clic en **siguiente**.
5. Ahora puede seleccionar la ubicación de la copia de seguridad que desea restaurar. Si la imagen es local, puede seleccionarla en la lista. 
6. Si la imagen se encuentra en un recurso compartido de red, seleccione **avanzadas**. También puede seleccionar **Opciones avanzadas** si necesita instalar un controlador.
   ![Server restore @ no__t-1
7. Si va a restaurar desde la red después de hacer clic en **avanzada** , seleccione **buscar una imagen de sistema en la red**. Es posible que se le pida que restaure la conectividad de red. Seleccione Aceptar. </br>
   ![Server restore @ no__t-1
8. Escriba la ruta de acceso UNC a la ubicación del recurso compartido de copia de seguridad (por ejemplo, \\ \ server1\backups) y haga clic en **Aceptar**. También puede escribir la dirección IP del servidor de destino, como \\ \ 192.168.1.3 \ backups. 
   ![Server restore @ no__t-1
9. Escriba las credenciales necesarias para tener acceso al recurso compartido y haga clic en Aceptar. 
10. Ahora **Seleccione la fecha y hora de la imagen de sistema que desea restaurar** y haga clic en **siguiente**.
11. Ahora se le ofrecerá una opción para:
    - Formatear y volver a particionar discos
    - Instalación de controladores
    - Anulación de la selección de las características **avanzadas** de reinicio automático y comprobación de errores de disco. Están habilitados de forma predeterminada.
12. Haz clic en **Siguiente**.
13. Haga clic en **Finalizar**. Se le preguntará si está seguro de que desea continuar. Haga clic en **Sí**.  
14. Una vez completado este paso, realice una restauración autoritativa de SYSVOL, como se describe en [recuperación del bosque de ad: realización de una sincronización autoritativa de SYSVOL replicada en DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Habilitar el adaptador de red para una copia de seguridad de red

Si necesita habilitar un adaptador de red desde el símbolo del sistema para restaurar desde un recurso compartido de red, siga estos pasos.

1. Inicie instalación de Windows, especifique el idioma, el formato de hora y moneda y las opciones de teclado y haga clic en **siguiente**. 
2. Haga clic en **Reparar el equipo**. CONFIGUR
3. Haga clic en **solucionar problemas**y haga clic en **símbolo del sistema**. 
4. Escriba el siguiente comando y presione ENTRAR:  

   ```  
   wpeinit  
   ```

5. Para confirmar el nombre del adaptador de red, escriba:  

   ```  
   show interfaces  
   ```  

   Escriba los siguientes comandos y presione ENTRAR después de cada comando:  

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

   Escriba `quit` para volver al símbolo del sistema. Escriba `ipconfig /all` para comprobar que el adaptador de red tiene una dirección IP e intente hacer ping en la dirección IP del servidor que hospeda el recurso compartido de copia de seguridad para confirmar la conectividad. Cuando haya terminado, cierre el símbolo del sistema. 

6. Ahora que el adaptador de red funciona, seleccione los pasos anteriores para completar la restauración.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
