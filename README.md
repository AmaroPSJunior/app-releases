# Repositório público de aplicativos Android

Este repositório é utilizado para **distribuir e atualizar aplicativos Android (APKs) já compilados e assinados** dos projetos desenvolvidos em repositórios privados.

Os dispositivos autorizados, como centrais multimídia Android, celulares e tablets, podem baixar as versões publicadas na seção **Releases**, diretamente pelo GitHub e sem precisar acessar o código-fonte.

## Como funciona

- O desenvolvimento, os testes e a compilação são feitos nos repositórios privados.
- Depois da validação, somente os arquivos APK destinados à distribuição são publicados aqui.
- Os aplicativos podem consultar as versões disponíveis e baixar atualizações pela internet.
- O código-fonte, as chaves de assinatura e as credenciais de acesso **não são publicados** neste repositório.

## Downloads

Acesse a seção [Releases](https://github.com/AmaroPSJunior/app-releases/releases) para encontrar os aplicativos disponíveis.

**Segurança:** instale apenas versões publicadas oficialmente. Este repositório é destinado a arquivos de distribuição, não ao desenvolvimento dos aplicativos.

## Canais de atualização por aplicativo

O diretório público `channels/` contém **um manifesto JSON por applicationId Android**, por exemplo
[`channels/org.example.chatgpttermuxcontroller.json`](channels/org.example.chatgpttermuxcontroller.json).
Cada app lê **somente o próprio manifesto**, sem depender da ordem global das Releases.

Contrato do manifesto (schema_version 1):
- `source_repository`: repositório de código que compilou o aplicativo (ex.: `AmaroPSJunior/chatgpt-termux-controller`).
- `application_id`: package name Android, que também identifica o caminho do canal.
- `signer_sha256`: SHA-256 do certificado X.509 que assinou o APK (não a chave privada).
- `version_name`, `version_code`, `tag_name`: identidade da versão publicada.
- `apk_url`, `apk_sha256`: URL pública exata e SHA-256 do binário.
- `source_commit`: commit de origem para rastreabilidade.

**Regras para os produtores:** publicar e verificar o APK assinado antes de atualizar o canal;
comparar version_code com o manifesto anterior e nunca voltar para uma versão inferior;
publicar cada projeto apenas no seu `application_id`. O publicador deve falhar se o
canal existente tiver outro `source_repository`, `application_id` ou `signer_sha256`.
A atualização do manifesto é a última etapa, depois da publicação do APK.

**Regras para os consumidores:** validar esquema, repositório de origem, package name,
fingerprint da assinatura, versão e URL esperados; baixar apenas por HTTPS, comparar
SHA-256, abrir os metadados do APK e confirmar o package name, versão/código e
compatibilidade da assinatura com o app já instalado antes de iniciar a instalação.
Um hash isolado não autentica a origem se o próprio manifesto puder ser adulterado:
a validação do certificado instalado/esperado é obrigatória.

O arquivo JSON permite que diferentes projetos publiquem no mesmo repositório sem
compartilhar um canal `latest`. Releases antigas continuam disponíveis no histórico.
