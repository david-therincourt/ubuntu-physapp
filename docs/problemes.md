# Problèmes résolus

## Erreur mise à jour noyau 6.11 et 6.14

L'installation des noyaux 6.11 et 6.14 pose problème lors de la mise à jour d'Ubuntu 24.04.

Etapes à suivre pour la désinstallation :

- Booter sur l'ancien noyau (ex. noyau 6.08) à partir du menu de grub.

- Désinstaller le nouveau noyau qui pose problème.
  
  ```bash
  # Par exemple :
  $ sudo apt update
  $ sudo apt remove linux-image-6.14.0-29-generic
  $ sudo apt remove linux-headers-6.14.0-29-generic
  $ sudo dpkg --configure -a   # Déjà fait avec apt remove
  ```

- Figer les paquets du noyau qui pose problème avec `apt-mark`.

- Rédémarrer.

## Figer un paquet avec `apt-mark`

Pour ignorer l'installation d'un paquet lors d'un mise à jour par exemple, il faut marquer le paquet comme figé (`hold`).

```bash
# Par exemple :
$ sudo apt-mark hold linux-image-6.14
linux-image-6.14.0-1004-oem passé en figé (« hold »).
linux-image-6.14.0-1005-nvidia passé en figé (« hold »).
...
linux-image-6.14.0-29-generic passé en figé (« hold »).

$ sudo apt-mark hold linux-headers-6.14
linux-headers-6.14.0-1004-oem passé en figé (« hold »).
linux-headers-6.14.0-1005-nvidia passé en figé (« hold »).
...
linux-headers-6.14.0-29-generic passé en figé (« hold »)
```

Pour lister le nom des paquets figés :

```bash
$ sudo apt-mark showhold
```

Pour annuler l'état figé d'un paquet :

```bash
$ sudo apt-mark unhold nom_paquet
```


