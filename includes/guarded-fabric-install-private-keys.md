Si no ha proporcionado un archivo PFX para el cifrado o firma de certificados en el primer servidor HGS, solo la clave pública se replicarán a este servidor.
Deberá instalar la clave privada mediante la importación de un archivo PFX que contiene la clave privada en el almacén de certificados local o, en el caso de claves respaldadas con HSM, configurar el proveedor de almacenamiento de claves y lo asocia con los certificados por el fabricante HSM instrucciones.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->