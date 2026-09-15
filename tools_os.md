# Windows

Comandos de linha de comando (CMD/PowerShell) para inspecionar e diagnosticar rede.

## 1. ipconfig

Mostra a configuração de rede da máquina local: IP atual, máscara de sub-rede, gateway padrão, servidores DNS, etc.

```
ipconfig
```

Variações úteis:
```
ipconfig /all          # mostra tudo, incluindo MAC address e DNS
ipconfig /release       # libera o IP atual (DHCP)
ipconfig /renew         # solicita um novo IP ao servidor DHCP
ipconfig /flushdns      # limpa o cache local de DNS
```

## 2. route print

Mostra a **tabela de rotas** da máquina: para quais redes de destino o tráfego deve ir, e por qual gateway/interface.

```
route print
```

É útil para entender por qual caminho o tráfego está saindo — por exemplo, se você tem duas placas de rede (Wi-Fi e Ethernet), essa tabela mostra qual delas é usada por padrão para acessar a internet (rota `0.0.0.0`).

## 3. ARP

O **ARP (Address Resolution Protocol)** resolve IP → MAC dentro de uma rede local. Antes de dois dispositivos na mesma rede trocarem dados, eles precisam saber o MAC um do outro (não só o IP).

```
arp -a
```

Mostra a tabela de IPs e MACs que sua máquina já "conhece" na rede local (cache ARP).

## 4. ping

Testa conectividade e latência até um destino, enviando pacotes ICMP e medindo o tempo de resposta.

```
ping google.com
```

Serve para checar: o destino está de pé? Existe perda de pacotes? Qual a latência (ms)?



# Linux
 
## 1. ip addr 
 
Mostra a configuração de rede: IP atual, máscara, interfaces disponíveis.
 
```
ip addr
```
 
Variações úteis:
```
ip addr show eth0         # detalhes de uma interface específica
ip link set eth0 up       # ativa a interface
ip link set eth0 down     # desativa a interface
resolvectl flush-caches   # limpa cache de DNS (systemd-resolved)
```
 
O comando `ifconfig` também existe, mas está **deprecado** na maioria das distros atuais — `ip` é o padrão moderno (pacote `iproute2`).
 
## 2. ip route
 
Mostra a tabela de rotas: para qual gateway/interface o tráfego de cada rede de destino deve ir.
 
```
ip route
```
 
A rota padrão (equivalente ao `0.0.0.0` do Windows) aparece como `default via <gateway> dev <interface>`.
 
O comando antigo `route -n` também funciona, mas `ip route` é o atual.
 
## 3. ARP 
 
Resolve IP → MAC dentro da rede local, igual no Windows.
 
```
ip neigh
```
 
(comando antigo equivalente: `arp -a`)
 
## 4. ping
 
Idêntico ao Windows, com uma diferença: no Linux o `ping` não para sozinho, ele continua até você apertar `Ctrl+C` (a não ser que use `-c`).
 
```
ping -c 4 google.com      # envia só 4 pacotes e para
```