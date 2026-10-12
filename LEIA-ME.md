# Site Dev Venturini — publicar na HostGator

Site estático (HTML + CSS, sem build, sem banco de dados). Vai para a pasta `public_html` da hospedagem.

## O que sobe e o que NÃO sobe

**Sobe** (tudo isso vai para dentro de `public_html`):

```
.htaccess            ← arquivo oculto, não esqueça dele
index.html
404.html
favicon.ico  favicon.svg  apple-touch-icon.png  icon-192.png  icon-512.png
site.webmanifest  robots.txt  sitemap.xml
assets/css/style.css
assets/fonts/        (as 5 fontes .woff2 e as licenças)
assets/img/          (joao.jpg, og-image.png, logo.svg, simbolo.svg)
```

**Não sobe:**
- `assets/joao.jpg` — a foto original, em tamanho cheio. A do site é `assets/img/joao.jpg`.
- `deploy/` — configurações de Nginx/Caddy, não servem na HostGator.
- `LEIA-ME.md` — este arquivo.

O `.htaccess` também bloqueia esses três caso subam por engano.

---

## Passo 1 — Domínio apontando para a HostGator

**Se você comprou o domínio na própria HostGator:** normalmente já vem apontado. Pule para o passo 2.

**Se o domínio está no Registro.br (ou outro lugar):**

1. Ache os **nameservers** da sua hospedagem. Vêm no e-mail de boas-vindas da HostGator ou no Portal do Cliente, nos dados do plano. São dois endereços parecidos com `nsXXX.hostgator.com.br`. Use exatamente os que aparecem para você.
2. No Registro.br: entre, clique em `devventurini.com` → **Alterar servidores DNS** → cole os dois nameservers → salve.
3. Se o domínio não foi informado na contratação do plano, adicione no cPanel em **Domínios** (ou "Domínios de complementos").
4. Espere a propagação: costuma levar algumas horas, no máximo 72h. Dá para acompanhar em dnschecker.org (tipo **NS**).

> Se você já usa algum e-mail com esse domínio em outro serviço, ele para de funcionar quando o DNS mudar, até você recriar os registros MX na HostGator. Como o `contato@` vai ser criado na própria HostGator (passo 5), isso não deve ser problema.

## Passo 2 — Subir os arquivos (Gerenciador de Arquivos)

1. No seu computador, entre na pasta `devventurini`, selecione **só os itens da lista "Sobe"** e compacte num `.zip` (botão direito → Compactar para arquivo ZIP).
   - O Windows esconde o `.htaccess` por padrão: no Explorador, **Exibir → Mostrar → Itens ocultos**.
   - Compacte o **conteúdo** da pasta, não a pasta em si. Dentro do zip, o `index.html` precisa estar na raiz.
2. Entre no **cPanel** → **Gerenciador de Arquivos** → abra `public_html`.
3. Se houver uma página padrão da HostGator (`index.html`, `default.php` ou parecido), apague.
4. **Carregar** → envie o `.zip`.
5. Clique com o botão direito no zip → **Extract** (Extrair) → confirme o destino `public_html` → depois apague o `.zip`.
6. Em **Configurações** (canto superior direito do Gerenciador), marque **Mostrar arquivos ocultos** e confira se o `.htaccess` está lá.

Estrutura final esperada:

```
public_html/
├── .htaccess
├── index.html
├── 404.html
├── favicon.ico ... (ícones)
└── assets/
    ├── css/style.css
    ├── fonts/...
    └── img/...
```

## Passo 3 — HTTPS (cadeado)

1. Nos planos atuais da HostGator o SSL gratuito costuma ser instalado sozinho depois que o domínio aponta. Abra https://www.devventurini.com: se aparecer o cadeado, está pronto.
2. Se não aparecer: no cPanel procure **SSL/TLS Status** e clique em **Run AutoSSL**, ou peça o SSL gratuito no Portal do Cliente/atendimento. Pode levar algumas horas depois da propagação do DNS.
3. **Só depois que o cadeado funcionar**, o redirecionamento do `.htaccess` passa a valer de verdade (http → https e sem www → com www). Antes disso o navegador pode avisar "site não seguro" — é normal enquanto o SSL não sai.

> Se o site entrar em "redirecionamentos demais" (erro ERR_TOO_MANY_REDIRECTS), apague as linhas do bloco **1** do `.htaccess` e me avise.

## Passo 4 — Testar

- [ ] https://www.devventurini.com abre com cadeado
- [ ] http://devventurini.com vai sozinho para https://www.devventurini.com
- [ ] No celular: todos os botões de WhatsApp abrem a conversa com a mensagem pronta
- [ ] https://www.devventurini.com/qualquer-coisa mostra a página 404 do site
- [ ] https://www.devventurini.com/assets/joao.jpg **não** abre (foto original protegida)
- [ ] Cole o link numa conversa do WhatsApp e veja a prévia com a imagem preta

---

## Depois de publicar

### 5. Criar o e-mail contato@devventurini.com
1. cPanel → **Contas de e-mail** → **Criar** → `contato` @ `devventurini.com` → senha forte.
2. Para ler no celular: no cPanel, na conta criada, **Connect Devices / Configurar cliente de e-mail** mostra os dados de IMAP/SMTP para colocar no Gmail ou no app de e-mail do celular.
3. Mande um e-mail de teste de outra conta para garantir que chega.

### 6. Google Search Console (aparecer no Google)
1. Acesse search.google.com/search-console → **Adicionar propriedade** → **Prefixo do URL** → `https://www.devventurini.com/`.
2. Verificação mais simples: **Arquivo HTML**. Baixe o arquivo que o Google gerar e suba em `public_html` pelo Gerenciador de Arquivos.
3. Depois de verificado: menu **Sitemaps** → envie `sitemap.xml`.
4. Em **Inspeção de URL**, cole o endereço do site e clique em **Solicitar indexação**.

### 7. Google Meu Negócio (aparecer no Maps em Londrina)
1. business.google.com → criar perfil "Dev Venturini".
2. Categoria: algo como "Designer de sites" ou "Empresa de software".
3. Como você não atende num endereço físico, escolha **atender clientes na região** e marque Londrina e região, sem mostrar endereço.
4. Coloque o site, o WhatsApp e a foto/logo do kit da marca.

### 8. Redes sociais
- Instagram @dev.venturini: coloque `https://www.devventurini.com` no link da bio.
- Na bio do kit estava "Teste grátis ↓". Troque por **"Fale comigo ↓"** ou **"Peça seu orçamento ↓"**, já que o site agora chama para contato.
- Publique o post de apresentação (`02-redes-sociais/post-apresentacao-1080x1350.png` do kit).

---

## Para alterar o site depois
- Textos e preços: `index.html` (procure o trecho e edite).
- Cores e fontes: topo do `assets/css/style.css`, em `:root`.
- Depois de editar: suba **só o arquivo alterado** pelo Gerenciador de Arquivos, substituindo o antigo.
- Se mudar conteúdo importante, atualize a data em `sitemap.xml` (`<lastmod>`).
