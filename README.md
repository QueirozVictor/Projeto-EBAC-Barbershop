# 💈 Barbearia Estilo Livre - Otimização de Performance Web

Este projeto é uma página institucional responsiva para a barbearia **Estilo Livre**, desenvolvida originalmente utilizando HTML5 estrutural, CSS customizado e componentes do framework Bootstrap 5. 

O objetivo principal deste repositório foi realizar uma auditoria completa de performance focada nas métricas do **Core Web Vitals (Google Lighthouse)**, identificando gargalos estruturais e aplicando técnicas avançadas de otimização de código e mídia sob o ecossistema do **Vite**.

---

## 📊 Comparativo de Performance (Antes vs. Depois)

Abaixo está o resumo da evolução das notas de auditoria geradas pelo Google Lighthouse:

| Categoria | Relatório Inicial (Legado) | Relatório Final (Otimizado) | Evolução |
| :--- | :---: | :---: | :---: |
| **Performance** | **70 / 100** | **100 / 100** | **+30 pontos** |
| **Acessibilidade** | 86 / 100 | 98 / 100 | **+12 pontos** |
| **Melhores Práticas** | 100 / 100 | 100 / 100 | Estável |
| **SEO** | 82 / 100 | 92 / 100 | **+10 pontos** |

### 📸 Evidências das Pontuações

#### 1. Relatório Inicial (Antes das Otimizações)
*Ambiente de teste: Localhost legado sem compilação.*

![Print da nota inicial - 70 Pontos](/src/img/RelatorioInicial.webp)

#### 2. Relatório Final (Após as Otimizações)
*Ambiente de teste: Build de produção otimizado rodando em `npm run preview`.*

![Print da nota final - 100 Pontos](/src/img/RelatorioFinal.webp)

---

## 🔍 Gargalos Encontrados na Primeira Avaliação

Na auditoria inicial, a página obteve nota **70 em Performance**, com um tempo crítico de carregamento do maior elemento visual (**LCP**) medido em alarmantes **28.4 segundos**. Os principais problemas técnicos mapeados foram:

1. **Mídias Superdimensionadas e Pesadas:** As imagens de fundo (`fundo-depoimento-1.JPEG` e `fundo-movel.JPEG`) somavam mais de **3.4 MB** sozinhas. Elas estavam no formato legado JPEG sem nenhuma compressão e com resoluções muito superiores ao tamanho real exibido na tela.
2. **Requisições Bloqueantes de Renderização (Render-blocking):** O site realizava a importação síncrona de um arquivo `style.css` de **269 KB** (contendo o Bootstrap inteiro compilado) e uma biblioteca JavaScript compactada inteira de **78 KB**, travando a primeira renderização do navegador por **1.610ms**.
3. **Instabilidade Visual (CLS Embutido):** Imagens críticas e ícones de redes sociais não possuíam atributos explícitos de tamanho (`width` e `height`) no HTML, fazendo os elementos do layout "pularem" durante o carregamento da página.
4. **Erros Graves de SEO:** Falta da tag `<meta name="description">` e ausência de atributos `alt` em imagens estruturais da galeria.

---

## 🛠️ Otimizações Aplicadas e Melhorias Implementadas

Para solucionar os problemas descritos acima, o ecossistema do **Vite** foi implementado juntamente com o pré-processador **Sass**. As seguintes ações foram tomadas:

### 1. Limitação e Importação Seletiva do Bootstrap (Sass)
Em vez de carregar a folha de estilos completa do framework, o arquivo `src/scss/custom.scss` foi configurado para importar **apenas as dependências estritas** utilizadas no design da barbearia:
* Core: `functions`, `variables`, `maps`, `mixins` e `utilities`.
* Componentes específicos: `containers`, `grid`, `nav`, `navbar`, `buttons`, `forms`, `card`, `carousel` e `transitions`.
* **Impacto:** O payload de CSS foi esmagado de **269 KB para apenas 21 KB**, eliminando o aviso de código não utilizado.

### 2. Otimização do Peso do JavaScript
Remoção do arquivo monolithic `bootstrap.bundle.min.js`. O ponto de entrada JavaScript (`src/js/main.js`) passou a gerenciar as importações assíncronas do ecossistema ESM:
* Importação exclusiva dos submódulos `carousel` e `collapse` do Bootstrap em conjunto com o `@popperjs/core`.
* **Impacto:** O peso do JS final em produção foi reduzido para meros **7.8 KB**.

### 3. Tratamento de Imagens e Estabilidade Visual
* **Conversão e Redimensionamento:** Todas as mídias da pasta `src/img/` foram convertidas para o formato moderno **.webp** de alta compressão e redimensionadas para as dimensões reais de exibição.
* **Estabilidade Estrutural via CSS:** Em vez de fixar dimensões rígidas no HTML que poderiam quebrar a fluidez responsiva, o problema do salto de layout (CLS) foi mitigado definindo propriedades de proporção e limites fluidos (`max-width: 100%` e `height: auto`) diretamente nas classes utilitárias de estilo, garantindo a estabilidade visual durante o carregamento de imagens e cards no mobile e desktop.
* **Carregamento Fluido:** Aplicação nativa de `loading="lazy"` nas imagens da galeria de serviços que ficam abaixo da dobra.

### 4. Ajustes de Acessibilidade e SEO
* Adicionada a tag `<meta name="description">` descritiva para indexação dos motores de busca.
* Inclusão do helper `@import "helpers/visually-hidden"` do Bootstrap para esconder de forma acessível os textos textuais `"Previous"` e `"Next"` das setas do carrossel, mantendo a leitura para leitores de tela sem quebrar a UI.
* Correção de atributos `alt` em toda a grade de imagens.

---

## 📈 Conclusão do Impacto

A migração do modelo monolítico legado para uma arquitetura modular moderna automatizada pelo **Vite** trouxe o maior impacto de performance para o projeto. 

A minificação agressiva automatizada no processo de build (`npm run build`) unificada com o corte cirúrgico de estilos mortos do Bootstrap reduziu o peso total da página de **mais de 7.5 MB para apenas 167 KB**. Isso garantiu uma experiência de carregamento instantânea (**FCP de 1.0s**) e estabilidade total de layout para o usuário final, atingindo com excelência todos os critérios técnicos exigidos.

---

## 💻 Como Executar o Projeto Localmente

1. Clone este repositório para a sua máquina.
2. Certifique-se de ter o **Node.js** instalado.
3. Abra o terminal na raiz do projeto e instale as dependências:
   ```bash
   npm install
   ```
4. Para rodar em modo de desenvolvimento com Hot Reload:
   ```bash
   npm run dev
   ```
5. Para testar o build de produção final otimizado (exatamente como avaliado no relatório final de 100 pontos):
   ```bash
   npm run build
   ```
   ```bash
   npm run preview
   ```
