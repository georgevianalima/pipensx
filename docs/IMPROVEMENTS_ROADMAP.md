# PIPENSX — Roadmap de melhorias

Este documento registra as melhorias incorporadas ao fork `georgevianalima/pipensx`.

A ideia é evoluir o projeto em passos pequenos, fáceis de testar e fáceis de desfazer.

## Como pensar no projeto

Imagine o PIPENSX como uma equipe:

- `TorboxClient`: conversa com a TorBox pela internet.
- `TorboxProvider`: transforma essa conversa no formato usado pelo restante do programa.
- `DownloadManager`: coordena os downloads.
- `Installer`: instala os pacotes.
- `AppSettings`: guarda as preferências.

Cada parte deve ter uma responsabilidade clara.

## Etapas planejadas

### 1. TorBox mais resistente

- reconhecer `DUPLICATE_ITEM` ao criar um torrent;
- procurar o torrent existente pelo hash;
- reutilizar torrents já existentes;
- adicionar retry apenas para falhas transitórias;
- invalidar informações antigas quando necessário;
- manter TLS seguro e nunca mostrar a API key nos logs.

### 2. Seleção inteligente de arquivos

- identificar NSP, NSZ, XCI e XCZ;
- separar `SKIP`, `DOWNLOAD` e `INSTALL`;
- evitar baixar arquivos auxiliares desnecessários;
- manter a seleção compatível com os providers.

### 3. Streaming e memória

- limitar a quantidade de dados mantida na memória;
- impedir que o produtor de dados fique muito à frente do instalador;
- usar `request gate` para controlar a pressão do buffer;
- recusar streaming quando a memória disponível for insuficiente.

### 4. Instalação recuperável

- registrar pontos seguros de progresso;
- permitir retomada quando for seguro;
- fazer rollback quando o estado não puder ser recuperado.

### 5. Configurações robustas

- versão do arquivo de configuração;
- validação dos tipos dos campos;
- escrita temporária seguida de troca segura;
- migração de configurações antigas.

### 6. Atualização e manutenção

- revisar o sistema de atualização;
- proteger URLs e tokens nos logs;
- melhorar mensagens de erro;
- registrar mudanças importantes no changelog.

## Ordem de implementação

1. TorBox: duplicados + busca por hash.
2. TorBox: retry e invalidação de informações antigas.
3. Seleção de arquivos.
4. Memória/streaming.
5. Journal e retomada da instalação.
6. Configurações.
7. Atualizador.

## Regra de segurança

Não copiamos código de outros forks cegamente. Primeiro entendemos a ideia e depois adaptamos à arquitetura atual do PIPENSX.

Não copiamos desativações de TLS. A comunicação HTTPS deve continuar validando o certificado do servidor.

## Guia para quem está aprendendo

Cada mudança importante deve explicar:

1. **O problema:** o que estava errado?
2. **A ideia:** como vamos resolver?
3. **Onde:** qual arquivo será alterado?
4. **Código:** o que mudou?
5. **Teste:** como sabemos que funcionou?

Assim, o projeto também funciona como material de estudo.

## Fontes das ideias

As melhorias são comparadas principalmente entre o PIPENSX atual, o P2PNX original da CNX17 e o fork P2PNX do usuário. O objetivo é reaproveitar ideias boas, não copiar código sem entender suas dependências.
