# 🎨 IKEMEN & MUGEN Stage Builder Studio

Uma ferramenta visual e intuitiva para criar cenários (stages) completos para **MUGEN 1.0**, **MUGEN 1.1** e **IKEMEN GO**, com compilação nativa de arquivos **SFF v1** — sem depender de softwares externos!

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Tkinter](https://img.shields.io/badge/GUI-Tkinter-green)
![Pillow](https://img.shields.io/badge/Pillow-Required-yellow)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 📑 Índice

1. [Funcionalidades](#-funcionalidades)
2. [Requisitos](#-requisitos)
3. [Instalação](#-instalação)
4. [Como Usar](#-como-usar)
5. [Controles do Preview](#️-controles-do-preview)
6. [Estrutura de Saída](#-estrutura-de-saída)
7. [Como o SFF v1 é Gerado](#-como-o-sff-v1-é-gerado)
8. [Estrutura do Código](#️-estrutura-do-código)
9. [Limitações Conhecidas](#️-limitações-conhecidas)
10. [Roadmap](#-roadmap)
11. [Contribuindo](#-contribuindo)
12. [Licença](#-licença)
13. [Agradecimentos](#-agradecimentos)
14. [Contato](#-contato)

---

## ✨ Funcionalidades

- 🖼️ **Preview visual interativo** — veja o cenário sendo montado em tempo real
- 🎯 **Definição da linha do chão (zoffset)** com clique do mouse no preview
- 🕹️ **Arraste a plataforma/chão** com o botão direito do mouse para ajuste fino
- 🎚️ **Sliders e spinboxes** para controle preciso de todos os parâmetros
- 🧩 **Compilador SFF v1 nativo** — 100% compatível com MUGEN 1.0, 1.1 e IKEMEN
- 🎨 **Dithering opcional** (Floyd-Steinberg) para suavizar degradês em paletas PCX
- 🎵 **Suporte a música de fundo** com loop configurável (`.mp3` / `.ogg`)
- 📐 **Resoluções pré-definidas**: 1280x720 (HD), 1920x1080 (Full HD) e 640x480 (4:3)
- 📦 **Geração automática de arquivos `.DEF` + `.SFF`** prontos para uso
- 🧠 **Cálculo automático de paralaxe e limites de câmera** baseado no tamanho das imagens
- 🌗 **Interface escura** estilo IDE para maior conforto visual

---

## 📋 Requisitos

- **Python 3.8+**
- **Pillow** (PIL)

> O Tkinter já vem incluso na instalação padrão do Python na maioria das distribuições.
> No Linux, caso não esteja instalado: `sudo apt install python3-tk`

---

## 🔧 Instalação

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/stage-builder-studio.git
cd stage-builder-studio
```

### 2. Instale as dependências

```bash
pip install pillow
```

### 3. Execute o aplicativo

```bash
python stagetool.py
```

Alternativamente, você pode baixar apenas o arquivo `stagetool.py` e executá-lo diretamente — ele é auto-contido e não depende de outros arquivos do projeto.

---

## 🚀 Como Usar

### Passo 1 — Preencha as configurações

| Campo | Descrição |
|-------|-----------|
| **Nome do Cenário** | Nome exibido no jogo (também usado para nomear os arquivos) |
| **Autor** | Seu nome ou apelido |
| **Resolução Alvo** | Escolha entre HD (1280x720), Full HD (1920x1080) ou 4:3 (640x480) |
| **Camada 0 (Fundo)** | Imagem de fundo (PNG/JPG/BMP) |
| **Camada 1 (Chão)** | Imagem do chão/plataforma (PNG/JPG/BMP) |
| **Música** | Faixa de fundo opcional (MP3/OGG) |
| **Loop Contínuo** | Marque para repetir a música indefinidamente |
| **Dithering** | Marque para suavizar gradientes nas imagens (recomendado para fundos com céu/degradês) |

### Passo 2 — Ajuste os parâmetros

- **Linha do Chão (zoffset):** Clique no preview com o **botão esquerdo** para definir onde os pés dos lutadores tocam o chão.
- **Distância Inicial Lutadores:** Distância em pixels entre P1 e P2 no início da luta.
- **Ajuste Fino da Plataforma (X/Y):** Use os sliders **X** e **Y** — ou **arraste com o botão direito** diretamente no preview para mover a plataforma de forma intuitiva.

### Passo 3 — Gere o cenário

Clique em **⚙️ GERAR CENÁRIO COMPLETO** e escolha a pasta `stages` do seu jogo. A ferramenta processará as imagens, compilará o `.SFF` e escreverá o `.DEF` automaticamente.

### Passo 4 — Registre no seu jogo

Adicione o cenário ao seu jogo. No MUGEN, o registro normalmente é feito no `select.def` do screenpack:

```
stages/MeuCenario/MeuCenario.def
```

No IKEMEN GO, adicione a linha no `select.def`:

```
stages/MeuCenario/MeuCenario.def, order=1
```

---

## 🖱️ Controles do Preview

| Ação | Efeito |
|------|--------|
| **Clique esquerdo / arrastar** | Define a linha do chão (zoffset) |
| **Clique direito / arrastar** | Move a plataforma horizontal e verticalmente |
| **Redimensionar janela** | O preview se adapta automaticamente |

---

## 📁 Estrutura de Saída

Após clicar em **Gerar Cenário**, a ferramenta cria a seguinte estrutura dentro da pasta escolhida:

```
stages/
└── MeuCenario/
    ├── MeuCenario.def     ← Definição do cenário (câmera, bounds, BGdef)
    ├── MeuCenario.sff     ← Sprites compilados (SFF v1)
    ├── bg.png             ← Fundo processado (redimensionado/cortado)
    ├── floor.png          ← Chão processado (redimensionado/ancorado na base)
    └── MeuCenario.mp3     ← Música (se fornecida, copiada com o nome do stage)
```

O arquivo `.def` gerado já contém:

- Seção `[Info]` com nome, display name e autor
- Seção `[Camera]` com limites calculados automaticamente a partir do tamanho das imagens
- Seção `[PlayerInfo]` com posições iniciais P1/P2 e bounds de tela
- Seção `[Bound]` com margens laterais
- Seção `[StageInfo]` com `localcoord`, `zoffset`, `autoturn`, `resetBG` e configurações de música
- Seção `[BGdef]` referenciando o `.sff`
- Duas camadas: `[BG Fundo]` (paralaxe calculada) e `[BG Chao]` (delta 1,1 com posição ajustada)

---

## 🧠 Como o SFF v1 é Gerado

O compilador nativo implementa o formato **Elecbyte Sprite File v1** conforme especificação oficial:

1. Cada imagem é convertida para **RGBA** e quantizada em **255 cores** usando o método `MEDIANCUT`.
2. O índice **0** da paleta é reservado para **transparência** (cor-chave preta).
3. Opcionalmente, aplica-se **dithering Floyd-Steinberg** para melhorar gradientes em imagens com degradê.
4. As imagens são salvas em **PCX** (formato suportado nativamente pelo MUGEN) e embutidas no `.sff`.
5. Cada sprite recebe um **cabeçalho de 32 bytes** com offset do próximo bloco, tamanho, eixos (axis) e grupo/índice.
6. Os offsets de eixo (**axis**) são calculados automaticamente: `(largura/2, 0)` para o fundo e `(largura/2, 0)` para o chão, garantindo alinhamento correto no jogo.

O cabeçalho principal do SFF (512 bytes) inclui a assinatura `ElecbyteSpr\0`, versão, contagem de grupos e sprites, e o nome do autor no campo de comentário.

---

## 🛠️ Estrutura do Código

```
stagetool.py
├── imagem_para_pcx()      → Converte PIL.Image em bytes PCX com paleta indexada
├── gerar_sff_v1()         → Compila o arquivo .SFF v1 completo
├── preparar_fundo()       → Redimensiona/corta o fundo para a resolução alvo
├── preparar_chao()        → Redimensiona/recorta o chão (ancorado na base)
└── StageBuilderApp        → Interface gráfica Tkinter
    ├── __init__()         → Inicializa variáveis e callbacks
    ├── setup_ui()         → Constrói a interface (painel esquerdo + preview)
    ├── add_entry()        → Helper para criar campos de texto
    ├── add_slider()       → Helper para criar sliders + spinboxes
    ├── add_file_picker()  → Helper para criar seletores de arquivo
    ├── on_res_change()    → Ajusta limites ao trocar resolução
    ├── load_bg()          → Carrega imagem de fundo
    ├── load_floor()       → Carrega imagem do chão
    ├── on_canvas_click()  → Define zoffset com clique esquerdo
    ├── on_right_click_press/drag() → Arrasta plataforma com botão direito
    ├── atualizar_posicao_chao()    → Recompõe base ao mover chão
    ├── compor_base()      → Compõe fundo + chão na imagem base
    ├── redesenhar()       → Atualiza o preview no canvas
    ├── posicionar_linha() → Desenha linha do chão e marcadores P1/P2
    ├── build_stage()      → Valida e inicia thread de geração
    ├── _process_build()   → Thread de geração dos arquivos finais
    └── _fim_build()       → Callback final (sucesso/erro)
```

---

## ⚠️ Limitações Conhecidas

- O compilador gera apenas **SFF v1** (compatível com MUGEN 1.0+, 1.1 e IKEMEN). Para SFF v2 (usado por MUGEN 1.1 com compressão PNG), seria necessário um compilador adicional.
- A resolução `640x480` pode exigir ajuste manual em `localcoord` para screenpacks específicos.
- Cenários com **múltiplas camadas de paralaxe** precisam ser editados manualmente no `.def` após a geração.
- O dithering aumenta o tempo de compilação em imagens muito grandes (1920x1080+).
- A música é copiada para a pasta do stage — arquivos muito grandes aumentam o tamanho final do pacote.

---

## 🗺️ Roadmap

- [ ] Suporte a **SFF v2** (PNG comprimido)
- [ ] Múltiplas camadas de paralaxe configuráveis via UI
- [ ] Importação de `.def` existentes para edição
- [ ] Preview animado com sprites de lutadores de teste
- [ ] Suporte a plataformas múltiplas com `[BG]` do tipo `parallax`
- [ ] Tradução para inglês
- [ ] Presets de configuração (salvar/carregar ajustes)
- [ ] Exportação de screenshot do preview

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se livre para:

1. Fazer um **fork** do projeto
2. Criar uma **branch** (`git checkout -b feature/minha-melhoria`)
3. **Commitar** suas alterações (`git commit -m 'Adiciona X'`)
4. **Push** para a branch (`git push origin feature/minha-melhoria`)
5. Abrir um **Pull Request**

Ao contribuir, mantenha o estilo de código existente e teste suas alterações com cenários reais em MUGEN e IKEMEN antes de enviar.

---

## 📜 Licença

Distribuído sob a licença **MIT**. Veja o arquivo `LICENSE` para mais informações.

---

## 🙏 Agradecimentos

- **Elecbyte** pela especificação do formato SFF
- **Comunidade MUGEN** pela documentação e testes contínuos
- **IKEMEN GO** por manter vivo o legado dos jogos de luta 2D
- **Pillow** pela biblioteca de manipulação de imagens

---

## 📧 Contato

Dúvidas, sugestões ou bugs? Abra uma **issue** no GitHub ou entre em contato pelo perfil do autor.

**Bom desenvolvimento e bons combates!** 🥊
