[← Voltar ao README](../README.md) · [Infraestrutura](INFRAESTRUTURA.md) · [Decisões técnicas](DECISOES.md)

# Infraestrutura

Este documento descreve onde e como o Cobot roda. Por segurança, ele não inclui endereços,
nomes de usuário, chaves ou qualquer dado de acesso ao servidor; essas informações ficam
apenas na documentação interna do clube.

## Visão geral

| Item | Escolha |
| --- | --- |
| Provedor | Oracle Cloud, plano Always Free |
| Sistema operacional | Ubuntu 24.04 |
| Linguagem | Python 3.12 |
| Disponibilidade | 24/7 |
| Gerenciamento do processo | systemd |
| Banco de dados | SQLite, em arquivo local na VM |
| Custo mensal | R$ 0 |

## Execução como serviço

O Cobot é registrado como um serviço do systemd. Na prática, isso garante três coisas:

1. **Inicia junto com a máquina.** Se a VM reiniciar, o bot volta sem ninguém precisar entrar no servidor.
2. **Reinicia após falhas.** Se o processo terminar de forma inesperada, o systemd o sobe de novo.
3. **Logs centralizados.** A saída do bot fica disponível pelo journal do sistema, o que facilita investigar problemas.

## Teste de resiliência

Para confirmar que o reinício automático funcionava, o processo do bot foi encerrado à força
em produção. O systemd detectou a queda e o Cobot voltou a responder no Discord em cerca de
**14 segundos**, sem intervenção manual.

## Persistência dos prazos

Os eventos ativos e seus prazos ficam gravados em SQLite, e não apenas na memória do
processo. Assim, se o bot reiniciar (por queda, atualização ou reinício da VM), ele recupera
a lista de eventos pendentes e continua removendo os posts no momento certo.

## Consumo

Em produção, o Cobot usa cerca de **32 MB** de memória, dentro dos limites da VM gratuita.

## Segurança

O comando de criação de eventos só é aceito no canal de staff do servidor, o que impede
publicações acidentais ou indevidas.

O raciocínio por trás de cada uma dessas escolhas está em [DECISOES.md](DECISOES.md).
