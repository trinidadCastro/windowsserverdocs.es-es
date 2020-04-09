---
title: 'Recuperación de bosque de AD: generación de grupos de RID'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: 308dce9be53194eb7db91944964ae5de03345ab6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823858"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Recuperación del bosque de AD: aumentar el valor de los grupos de RID disponibles 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Utilice el siguiente procedimiento para elevar el valor de los grupos de IDENTIFICADOres relativos (RID) que el maestro de operaciones de RID asignará después de que se restaure el controlador de dominio. Al aumentar el valor de los grupos de RID disponibles, puede asegurarse de que ningún controlador de dominio asigna un RID para una entidad de seguridad que se creó después de la copia de seguridad que se usó para restaurar el dominio. 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Acerca de los grupos de RID de Active Directory y rIDAvailablePool

Cada dominio tiene un objeto **CN = RID Manager $, CN = System, DC**=<*domain_name*>. Este objeto tiene un atributo denominado **rIDAvailablePool**. Este valor de atributo mantiene el espacio global de RID para un dominio completo. El valor es un entero grande con las partes superior e inferior. La parte superior define el número de entidades de seguridad que se pueden asignar para cada dominio (0x3FFFFFFF o simplemente en 1 mil millones). La parte inferior es el número de RID que se han asignado en el dominio. 
  
> [!NOTE]
> En Windows Server 2016 y 2012, el número de entidades de seguridad que se pueden asignar aumenta a más de 2 mil millones. Para obtener más información, consulte Administración de la [emisión de RID](https://technet.microsoft.com/library/jj574229.aspx). 
  
- Valor de ejemplo: 4611686014132422708  
- Parte baja: 2100 (principio del siguiente grupo RID que se va a asignar)  
- Parte superior: 1073741823 (número total de RID que se pueden crear en un dominio)  
  
Al aumentar el valor del entero grande, se aumenta el valor de la parte baja. Por ejemplo, si agrega 100.000 al valor de ejemplo de 4611686014132422708 para una suma de 4611686014132522708, la nueva parte baja es 102100. Esto indica que el siguiente grupo de RID que se asignará mediante el maestro RID comenzará con 102100 en lugar de 2100. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Para elevar el valor de los grupos de RID disponibles mediante ADSIEdit y la calculadora

1. Abra Administrador del servidor, haga clic en **herramientas** y en **Editor ADSI**.
2. Haga clic con el botón secundario en, seleccione **conectar con** y conecte el contexto de nomenclatura predeterminado y haga clic en **Aceptar**.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Vaya a la siguiente ruta de acceso de nombre distintivo: **CN = RID Manager $, CN = System, DC =<domain name>** .
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. Haga clic con el botón derecho y seleccione las propiedades de CN = RID Manager $. 
4. Seleccione el atributo **rIDAvailablePool**, haga clic en **Editar**y, a continuación, copie el valor entero grande en el portapapeles.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. Inicie Calculator y, en el menú **Ver** , seleccione **modo científico**. 
6. Agregue 100.000 al valor actual.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. Con Ctrl-c o el comando **copiar** del menú **edición** , copie el valor en el portapapeles. 
8. En el cuadro de diálogo Editar de ADSIEdit, pegue este nuevo valor. 
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Haga clic en **Aceptar** en el cuadro de diálogo y **aplíquelo** en la hoja de propiedades para actualizar el atributo **rIDAvailablePool** . 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Para elevar el valor de los grupos de RID disponibles mediante LDP  
  
1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  
   **LPD**  
2. Haga clic en **conexión**, haga clic en **conectar**, escriba el nombre del administrador de RID y, a continuación, haga clic en **Aceptar**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. Haga clic en **conexión**, haga clic en **enlazar**, seleccione **enlazar con credenciales** , escriba sus credenciales administrativas y, a continuación, haga clic en **Aceptar**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. Haga clic en **Ver**, en **árbol** y, a continuación, escriba la siguiente ruta de acceso distintivo: CN = RID Manager $, CN = System, DC =*Domain Name*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. Haga clic en **examinar**y, a continuación, en **modificar**. 
6. Agregue 100.000 al valor actual de **rIDAvailablePool** y, a continuación, escriba la suma en **los valores**. 
7. En **DN**, escriba `cn=RID Manager$,cn=System,dc=` *< nombre de dominio\>* . 
8. En **Editar atributo de entrada**, escriba `rIDAvailablePool`. 
9. Seleccione **reemplazar** como la operación y, a continuación, haga clic en **entrar**.
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Haga clic en **Ejecutar** para ejecutar la operación. Haga clic en **Cerrar**.
11. Para validar el cambio, haga clic en **Ver**, en **árbol**y escriba la siguiente ruta de acceso de nombre distintivo: CN = RID Manager $, CN = System, DC =*Domain Name*.   Compruebe el atributo **rIDAvailablePool** . 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
