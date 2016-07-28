# Contenedores Linux

## Introducción

> **lxc** es una tecnología de virtualización que permite crear máquinas a nivel Sistema Operativo para linux. Se basa en la funcionalidad cgrups de Linux que se encuentra disponible a partir del kernel 2.6.29

## Instalación

> 1. Primero se tiene que instalar *lxc*

```bash
  $ sudo apt-get install lxc
```
> 2. Checamos que el host soporte la tecnología de contenedores

```bash
  $ lxc-checkconfig
```

![lxc-checkconfig](url)

> 3. Creamos un contenedor con el comando lxc-create -n nombreDelContenedo -t template

```bash
  $ sudo lxc-create -n contenedor -t download
```

![lxc-checkconfig](url)

> 4. Iniciamos el contenedor con lxc-start -n nombreDelContenedor, al agregarle la opción *-d* le indicamos que inicie como demonio

```bash
  $ sudo lxc-start -n contenedor -d
```

> 5. Nos conectamos al contenedor iniciada lxc-attach -n nombreDelContenedor

```bash
  $ sudo lxc-attach -n contenedor
```

> 6. Para poder apagar el contenedor usamos el comando *poweroff*

```bash
  contenedor$ poweroff
```
> 7. Para poder obtener el estado de los contenedores utilizamos lxc-ls

```bash
  $ lxc-ls --fancy
```

> 8.- Para poder eliminar un contenedor se utiliza lxc-destroy -n nombreDelContenedor

```bash
  $ lxc-destroy -n contenedor
```

## Preguntas

* ¿Pueden detectar como se ven diferentes procesos init desde el anfitrión?

> Los procesos init de los contenedores se ven como procesos de */sbin/init*

* ¿Pueden detectar cuantos y cuales procesos se ven desde el invitado?

> Desde la máquina host solo se pueden ver los procesos que se estan ejecutando pero no se puede ver que contenedor lo ejecuta.
> De lado del contenedor solo se ven los procesos que él mismo está ejecutando

* ¿Qué otras llamadas al sistema importantes creen que pasan para inicializar el contenedor?

> clone, exec, load, write, read