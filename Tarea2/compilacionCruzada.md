# Compilación cruzada

## Introdución

Una compilación cruzada se refiere a la compilación de un programa bajo una arquitectura que genera un codigo de arquitectura diferente.

## Configuración

**Se realizará este ejemplo creando codigo para una raspberry pi 3**

1. Debemos obtener el hash del último commit de la imagen utilizada de raspberry. Se ejecuta lo siguiente en la raspberry pi

```bash
    $ zgrep "* firmware as of" /usr/share/doc/raspberrypi-bootloader/changelog.Debian.gz | head -1 | awk '{ print $5 }'
    d4f5315cfac4e
``` 

2. Ahora, dentro del host huesped, obtenemos el hash del firmware

```bash
    $ git clone https://github.com/raspberrypi/firmware
    $ cd firmware
    $ git checkout d4f5315cfac4e
    $ git reset --hard
    $ cat extra/git_hash
    1587f775d0a3c437485262ba951afc5e30be69fa
```

3. Obtenemos las herramientas necesarias para la compilacion con las raspberry

```bash
    $ git clone https://github.com/raspberrypi/tools
```

4.- Posteriormente descargamos el kernel de linux y seleccionamos la version que corresponda al hash obtenido

```bash
    $ git clone https://github.com/raspberrypi/linux.git
    $ cd liux
    $ git checkout 1587f775d0a3c437485262ba951afc5e30be69fa
    $ git reset --hard
```

5.- Ahora copiamos config.gz de la raspberry al host huesped. Dentro de la raspberry ejecutamos

```bash
    $ scp pi@ipderaspberry:/proc/config.gz .
```

6.- Descomprimimos el archivo obtenido en el host huesped

```bash
    $ zcat config.gz > .config
```

7.- En el host huesped ejecutamos

```bash
    $ make ARCH=arm CROSS_COMPILE=/path/donde/esta/tools/arm-bcm2708/arm-bcm2708-linux-gnueabi/bin/arm-bcm2708-linux-gnueabi- oldconfig
    $ make ARCH=arm CROSS_COMPILE=/path/donde/esta/tools/arm-bcm2708/arm-bcm2708-linux-gnueabi/bin/arm-bcm2708-linux-gnueabi-
```

8.- Terminado esto, podemos comenzar a compilar de la siguiente manera

```bash
    $ make ARCH=arm CROSS_COMPILE=/path/donde/esta/tools/arm-bcm2708/arm-bcm2708-linux-gnueabi/bin/arm-bcm2708-linux-gnueabi- -C /path/donde/esta/el/kernel M=$(PWD) modules
```