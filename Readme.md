# Ifupdown2

## Introduction

L'utilisation de ifupdown2 est maintenant généralisée sur les Lisos car nous
avions un problème avec ifupdown1 et la parallélisation des actions sur les
interfaces (par exemple impossible de mettre une interface QinQ dans un
bridge). Il a donc été décider d'utiliser ifupdown2.

Ce paquet permet de corriger un certain nombre de bugs ou comportement non
compatible avec Lisos.

* La gestion des addons est dans un autre paquet : ifupdown2-addons[^4]
* La gestion des namespaces est dans un autre paquet : ifupdown-netns[^5]

## Upstream

Précédemment basé sur une version avancé de 2.0.1[^1] d'ifupdown2 (buster)
avec la PR 138[^3] (ifupdown2-addons <= v0.3.3).

La version upstream actuelle (bullseye) de ifupdown2 est la 3.0.0-1[^2].

> /!\ la PR 138 n'est pas merge-able dans l'état actuelle mais doit être
> disponibles.

Toutes les corrections de bugs (ou contournement) sont faite dans ce paquet
en utilisant dpkg-divert pour changer le fichier source du paquet ifupdown2.

## Addons

Voir ifupdown2-addons[^4]

## Patches IfUpDown2

### Alphalink

Nous avons été obligé de faire plusieurs patchs pour corriger des bugs pour
adapter des fonctionnements par rapport à notre utilisation.

* lib/__nlcache__

Functions `addr_add_dry_run` et `addr_add`: Ajout d'un patch pour ajouter le
paramètre broadcast sur l'ajout d'une IP sur un réseau IPv4 supérieur à /31.

Function `bridge_is_vlan_aware`: Ajout d'un patch pour éviter que ifupdown2
pense que le bridge est un bond. Ce bug semble lié à l'inclusion de la pull
request 138.

### Pull Requests

Nous avons été obligé d'appliquer des pull requests avant leurs mise en place
dans l'upstream.

* ARP IP bond PR[^3]

Rend l'utilisation d'ARP IP possible pour les checks de bonds. exemple:
```
auto bond0
iface bond0 inet
bond-slaves ens21 ens22
bond-mode balance-rr
bond-arp-interval 100
bond-arp-ip-target 8.8.8.8
address 10.10.10.1/24
```

## Liens

[^1]: upstream commit v2.0.1+  46c2e97909a71b5a46bc926fd3b1d8010dce9705
[^2]: upstream commit v3.0.0-1 288a88d3e432c312c684c435eff9104be1f22953
[^3]: ARP IP bond PR https://github.com/CumulusNetworks/ifupdown2/pull/138
[^4]: ifupdown2 addons https://gitlab.alphalink.tech/lisos/ifupdown2-addons
[^5]: ifupdown netns https://gitlab.alphalink.tech/lisos/ifupdown-netns
