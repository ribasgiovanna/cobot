[← Voltar ao README](../README.md) · [Infraestrutura](INFRAESTRUTURA.md) · [Decisões técnicas](DECISOES.md)

# Decisões técnicas

Registro das principais escolhas feitas no Cobot e do motivo de cada uma.

## 1. Slash command em vez de formulário externo

**Decisão:** a criação de eventos acontece por um slash command (`/evento criar`) dentro do próprio Discord.

**Por quê:** a equipe do clube já se organiza no Discord. Um comando com campos definidos
evita trocar de ferramenta e garante que todo evento tenha as mesmas informações, no mesmo formato.

## 2. Comando restrito ao canal de staff

**Decisão:** `/evento criar` só funciona no canal da equipe.

**Por quê:** qualquer post criado pelo bot aparece para todos os membros do servidor.
Restringir o comando evita publicações acidentais ou indevidas.

## 3. SQLite para guardar os prazos

**Decisão:** eventos ativos e prazos são gravados em um banco SQLite local.

**Por quê:** se os prazos ficassem só na memória, qualquer reinício do bot faria os posts
pendentes ficarem no ar para sempre. O SQLite resolve isso sem exigir um servidor de banco
de dados separado, o que mantém a infraestrutura simples e gratuita.

## 4. Verificação periódica a cada 30 segundos

**Decisão:** o bot confere os prazos em um ciclo de 30 segundos.

**Por quê:** para prazos de inscrição, meio minuto de precisão é mais do que suficiente,
e um intervalo curto como esse tem custo desprezível de processamento.

## 5. Migração da Discloud para a Oracle Cloud

**Decisão:** o bot saiu da Discloud e passou a rodar em uma VM do plano Always Free da Oracle Cloud.

**Por quê:** o plano gratuito da Discloud ficou sem vaga. A Oracle oferece uma VM gratuita
e permanente, que dá controle total do ambiente e mantém o custo do clube em zero.

## 6. systemd para manter o bot no ar

**Decisão:** o Cobot roda como serviço do systemd.

**Por quê:** numa VM própria, não há plataforma cuidando do processo. O systemd reinicia o
bot automaticamente após falhas e após reinícios da máquina, comportamento que foi
confirmado com um teste de queda forçada (volta em cerca de 14 segundos).

## 7. Prazo exibido com o horário nativo do Discord

**Decisão:** o fim das inscrições aparece no post com o formato de horário nativo do Discord.

**Por quê:** o Discord converte esse horário para o fuso de cada pessoa automaticamente.
Assim ninguém precisa fazer conta de fuso para saber até quando pode se inscrever.
