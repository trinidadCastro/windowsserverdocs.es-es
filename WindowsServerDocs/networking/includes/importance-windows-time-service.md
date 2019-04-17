## <a name="importance-of-time-protocols"></a>Importancia de los protocolos de tiempo
Protocolos de tiempo que se comunican entre dos equipos para intercambiar información de tiempo y, a continuación, usar esa información para sincronizar sus relojes. Con el protocolo de tiempo del servicio hora de Windows, un cliente solicita información de tiempo desde un servidor y sincroniza su reloj según la información que se recibió.
  
El servicio hora de Windows usa NTP para sincronizar la hora a través de una red. NTP es un protocolo de tiempo de Internet que incluya los algoritmos disciplina necesarios para sincronizar el reloj. NTP es un protocolo de tiempo más preciso que el protocolo de tiempo de red Simple (SNTP) que se usa en algunas versiones de Windows; Sin embargo, W32Time continúa admitiendo SNTP para habilitar la compatibilidad con equipos que ejecutan servicios basados en SNTP tiempo como Windows 2000.

Hay muchas razones distintas, es posible que necesites tiempo preciso.  El caso habitual de Windows es Kerberos, lo que requiere 5 minutos de precisión entre el cliente y servidor.  Sin embargo, hay muchas otras áreas que pueden verse afectados por tiempo precisión que incluyen:


- Las reglamentaciones gubernamentales, como:
    - Precisión de 50 ms para FINRA en Estados Unidos
    - 1 ESMA de ms (MiFID II) en la Unión Europea.
- Algoritmos de cifrado
- Sistemas distribuidos como SQL de clúster o Exchange y bases de datos de documento
- Marco de Blockchain para las transacciones de bitcoin
- Registros distribuidos y análisis de amenazas 
- Replicación de AD
- PCI (sector de tarjeta de pago), precisión segunda actualmente 1