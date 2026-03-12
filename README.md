# DOCUMENTACAO DE AUDITORIA DE SEGURANCA: FORCA BRUTA E PASSWORD SPRAYING
---

## OBJETIVO DO PROJETO
Este projeto pratico tem como objetivo simular cenarios reais de ataques de quebra de credenciais em um ambiente laboratorial controlado. A finalidade e compreender a mecanica de ataques de Forca Bruta e Pulverizacao de Senhas (Password Spraying) contra multiplos servicos de rede, documentando o processo de exploracao e propondo medidas efetivas de mitigacao para proteger sistemas corporativos.

---

## ARQUITETURA DO AMBIENTE
O laboratorio foi construido utilizando o VirtualBox, configurado em uma rede isolada do tipo Host-Only para garantir a contencao e seguranca dos testes.
Maquina Atacante: Kali Linux.
Maquina Alvo: Metasploitable 2, um servidor intencionalmente vulneravel.

---

## FERRAMENTAS UTILIZADAS
Nmap: Para mapeamento de portas e descoberta de servicos.
Medusa: Para execucao de forca bruta rapida em protocolos de rede (FTP e SMB).
Hydra: Para exploracao de formularios web.
Wordlists: Criacao de listas customizadas via terminal.

---

## METODOLOGIA E EXECUCAO DOS ATAQUES

[Clique aqui para acessar a pasta com as evidências](./images)

### FASE 1: RECONHECIMENTO (INFORMATION GATHERING)
Antes de iniciar a exploracao, foi executada uma varredura de portas no alvo utilizando o Nmap para mapear a superficie de ataque. A varredura confirmou que as portas 21 (FTP), 80 (HTTP) e 139/445 (SMB) estavam abertas e suscetiveis a testes de autenticacao. (Imagem 1)

### FASE 2: CRIACAO DE MUNICIONAMENTO (WORDLISTS)
Foram gerados arquivos de texto contendo possiveis usuarios (users.txt) e senhas (pass.txt) diretamente via linha de comando, simulando o uso de credenciais padrao e informacoes coletadas em vazamentos de dados. (Imagem 2)



### CENARIO 1: FORCA BRUTA NO SERVICO FTP
O primeiro vetor de ataque foi o servidor FTP (vsftpd) rodando na porta 21. Utilizando a ferramenta Medusa, os arquivos de usuarios e senhas foram cruzados em um ataque de forca bruta tradicional. A ferramenta obteve sucesso ao identificar rapidamente as credenciais padrao do sistema.(Imagem 3 e Imagem 4)


### CENARIO 2: PASSWORD SPRAYING NO SERVICO SMB
Para o servico de compartilhamento de arquivos SMB, a estrategia foi alterada para um ataque furtivo de Password Spraying. Em vez de testar varias senhas contra um usuario, uma unica senha comum foi testada contra toda a lista de usuarios para evitar o acionamento de bloqueios temporarios de conta. O ataque foi bem-sucedido. (Imagem 5)


### CENARIO 3: ATAQUE WEB NO DVWA (TROUBLESHOOTING E EXPLORACAO)
O alvo final foi a pagina de login do DVWA (Damn Vulnerable Web App) hospedada no servidor web (porta 80). Durante os testes com o modulo web-form, a aplicacao retornou um codigo HTTP 302 (Redirecionamento).Com o ambiente estabilizado, a arquitetura do ataque foi ajustada para mapear corretamente os campos invisiveis do HTML e a mensagem de falha, resultando na quebra do acesso web. (Imagem  6 e Imagem 7)

---

## MEDIDAS DE MITIGACAO E PREVENCAO
A exploracao bem-sucedida destes vetores demonstra falhas criticas de configuracao. Para proteger a arquitetura contra esses ataques, recomendam-se as seguintes implementacoes:

- Politicas de Senha: Exigir complexidade e rotatividade de senhas, proibindo sumariamente o uso de credenciais padrao de fabrica nos deploys.
- Account Lockout (Bloqueio de Conta): Configurar o bloqueio automatico de usuarios apos um numero predeterminado de tentativas falhas de login (ex: 5 tentativas), neutralizando ataques de forca bruta tradicionais.
- Autenticacao Multifator (MFA): Implementar verificacao em duas etapas, especialmente em aplicacoes web, tornando senhas vazadas inuteis isol--adamente.
- Protecao de Formularios Web: Inserir CAPTCHAs em telas de autenticacao para inviabilizar a automacao por ferramentas como Medusa e Hydra.
- Monitoramento: Implementar sistemas de protecao ativa (como o Fail2Ban) para analisar logs do servidor, detectar e bloquear conexoes que gerem trafego anomalo de tentativas de acesso.
