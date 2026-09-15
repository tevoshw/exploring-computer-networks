# 1. O que é Rota

Uma rota é uma instrução que diz ao sistema: "para alcançar essa rede de destino, envie o pacote por esse caminho". Toda rota tem 3 componentes principais.

## 1.1 Rede de destino

É a faixa de IPs para a qual aquela rota vale. Ela é escrita em notação CIDR, ex: `192.168.1.0/24`, indicando um bloco inteiro de endereços — não um IP único.

Existe uma rede de destino especial: `0.0.0.0/0`, chamada de **rota padrão** (default route). Ela é o "coringa": se nenhuma outra rota mais específica combinar com o destino do pacote, essa é usada. É basicamente o "se eu não sei pra onde mandar, manda pra internet através do roteador".

O sistema sempre escolhe a rota **mais específica** disponível (a de prefixo mais longo, ex: `/24` antes de `/0`), e só cai na rota padrão como último recurso.

## 1.2 Gateway

É o IP do próximo dispositivo (geralmente um roteador) que vai receber o pacote e decidir o que fazer com ele em seguida — o "próximo salto" (next hop).

Seu computador não sabe o caminho inteiro até o destino final, só sabe entregar para o gateway. O gateway, por sua vez, tem sua própria tabela de rotas e decide para onde mandar em seguida. É esse repasse em cadeia, gateway após gateway, que forma o caminho até o destino — e é exatamente o que aparece em cada linha do `tracert`/`traceroute`.

Numa rede doméstica, o gateway padrão normalmente é o próprio roteador (ex: `192.168.0.1`).

## 1.3 Interface

É a placa de rede física (ou virtual) pela qual o pacote deve sair da máquina — Wi-Fi, Ethernet, uma VPN, etc.

Isso importa principalmente quando um dispositivo tem **mais de uma interface** de rede ativa ao mesmo tempo (ex: notebook conectado por Wi-Fi e Ethernet simultaneamente). Nesse caso, a rota também define *por qual placa* o pacote deve sair, além de para qual gateway.

Se você olhar a saída de `route print` (Windows) ou `ip route` (Linux), cada rota mostra justamente essas 3 informações lado a lado: destino, gateway e interface.