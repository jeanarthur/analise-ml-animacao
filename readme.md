# Análise do modelo de Machine Learning

## Modelo para geração de animações 2D - Toon Crafter

### **Visão Geral**

O **ToonCrafter** é uma ferramenta de interpolação generativa voltada para animações, que utiliza técnicas avançadas de aprendizado de máquina, especialmente modelos de difusão. Foi desenvolvido por uma equipe de pesquisadores associados à **[CUHK (Chinese University of Hong Kong)](https://www.cuhk.edu.hk/english/index.html)** e ao **[Tencent AI Lab](https://ailab.tencent.com/ailab/en/index/)** e apresentado como parte do **[SIGGRAPH Asia 2024 (Journal Track)](https://asia.siggraph.org/2024/presentation/?id=papers_156&sess=sess135)**, um importante evento de computação gráfica e técnicas interativas.

<table class="center">
    <tr style="font-weight: bolder;text-align:center;">
        <td>Imagem de Entrada</td>
        <td>Imagem de Saída</td>
        <td>Animação Gerada</td>
    </tr>
    <tr>
        <td>
            <img src=images/sample01_start.png width="250">
        </td>
        <td>
            <img src=images/sample01_end.png width="250">
        </td>
        <td>
            <img src=images/sample01_result.gif width="250">
        </td>
    </tr>
    <tr>
        <td>
            <img src=images/sample02_start.png width="250">
        </td>
        <td>
            <img src=images/sample02_end.png width="250">
        </td>
        <td>
            <img src=images/sample02_result.gif width="250">
        </td>
    </tr>
    <tr>
        <td>
            <img src=images/sample03_start.png width="250">
        </td>
        <td>
            <img src=images/sample03_end.png width="250">
        </td>
        <td>
            <img src=images/sample03_result.gif width="250">
        </td>
    </tr> 
    <tr>
        <td>
            <img src=images/sample04_start.png width="250">
        </td>
        <td>
            <img src=images/sample04_end.png width="250">
        </td>
        <td>
            <img src=images/sample04_result.gif width="250">
        </td>
    </tr>
</table>

---
### **Contexto e Problemas Abordados**

#### **Motivações para o Projeto**

O ToonCrafter foi criado com intuito de otimizar a produção de animações, acelerando partes demoradas como a necessidade de desenhar manualmente quadros intermediários.
A ideia era utilizar modelos de geração de vídeo a partir de imagens para criar os quadros intermediários.

#### **Problemas Abordados**

1. **Características das animações**:
   - **Esparsidade temporal**: Quadros são desenhados manualmente, o que pode resultar em grandes diferenças de movimento entre quadros consecutivos se não gasto tempo suficiente na construção das imagens.
   - **Estilo visual distinto**: Contornos bem definidos, cores sólidas e ausência de borrões de movimento, comuns em vídeos reais.
- Essas características dificultam a aplicação de métodos tradicionais de interpolação baseados em movimento linear.

2. **O problema do "gap de domínio"**:
  - Modelos treinados em vídeos reais podem interpretar de forma incorreta os estilos de animação, gerando conteúdo irrelevante ou inadequado.
    - Modelos tradicionais podem criar partes "realistas" em quadros de animação, como texturas ou movimentos que não condizem com o estilo animado.

3. **Limitações das Abordagens Tradicionais**:
   - Presumem movimentos simples e lineares, falhando em capturar fenômenos como **oclusões** (elementos cobrindo outros) e movimentos não-lineares.
   - Métodos baseados em correspondência de quadros, como fluxo óptico, são pouco eficazes em texturas simples e estruturas exageradas das animações.
   - Perdem detalhes em áreas críticas devido à compressão usada nos modelos generativos.

4. **Desafios na Decodificação**:
  - Os modelos de difusão trabalham em espaços comprimidos (latentes), sacrificando detalhes ao reconstruir imagens.
    - Isso é crítico em animações, onde as diferenças visuais são mais perceptíveis devido à ausência de borrões de movimento.

---
### **Soluções**

A solução combina pesquisa acadêmica e experiências práticas e industriais, promovendo avanços em **inteligência artificial generativa** aplicados à criação artística. 

#### As contibuições técnicas incluem:

#### **1. Toon Rectification Learning** 
Adapta os modelos de aprendizado treinados em vídeos reais para o domínio das animações.

  - Camadas responsáveis pelo entendimento do contexto visual e geração de conteúdo foram ajustadas especificamente para dados de animações, utilizando um conjunto de dados dedicado com mais de **500 horas de vídeos animados**.
  - Durante o ajuste fino (fine-tuning), **camadas temporais foram congeladas**, preservando o aprendizado de movimentos realistas já obtido em vídeos reais, enquanto as camadas espaciais foram treinadas para adaptar os detalhes visuais ao estilo animado.

#### **2. Decodificador 3D Baseado em Duas Referências**
Melhora a qualidade visual dos quadros gerados, compensando a perda de detalhes.

  - Um decodificador que combina informações detalhadas dos quadros inicial e final (referências duplas).
  - Utiliza:
    - **Atenção cruzada**: Integra detalhes dos quadros de entrada em camadas iniciais da decodificação.
    - **Aprendizado residual**: Corrige erros em camadas mais profundas, adicionando informações refinadas diretamente dos quadros de entrada.

#### **3. Codificador de Esboço Independente**
Oferece controle interativo, permitindo que os usuários ajustem o movimento gerado.

  - Um **codificador de esboços** interpreta desenhos fornecidos pelo usuário e os incorpora na interpolação.
  - Suporta esboços esparsos (não é necessário desenhar para todos os quadros), reduzindo a carga de trabalho do usuário.
  - Torna o modelo mais flexível para aplicações práticas.

#### **Detalhes Técnicos Adicionais**
- O desenvolvimento utilizou modelos de difusão latente, aproveitando arquiteturas existentes (como a **Stable Diffusion**) e aprimorando-as para tarefas específicas de interpolação.
- Ferramentas como **PyTorch** foram empregadas no treinamento e ajuste dos modelos.
- A equipe lançou os códigos, pesos do modelo pré-treinado e instruções de instalação para a comunidade open source no GitHub, incentivando colaborações e melhorias futuras.

#### **Resultados**

1. Maior consistência

    <img src=images/result_3d_decoder_comparison.png>

2. Melhores resultados

    <img src=images/result_interpolation_comparison.png>

3. Funcionalidades

    <img src=images/result_features.png>

Mais demostrações em: [ToonCrafter - GitHub Projects](https://doubiiu.github.io/projects/ToonCrafter/)

---
### **Limitações**

- Os vídeos gerados são relativamente curtos (2 segundos, FPS=8).
- O modelo tem dificuldade em renderizar texto legível.
- A parte de codificação automática do modelo tem perdas relevantes, resultando em pequenos artefatos e aberrações.

---
### **Formas de Uso**

#### Local
- [GitHub] Código fonte com instruções de instalação: [ToonCrafter](https://github.com/Doubiiu/ToonCrafter).
- [GitHub] Código fonte com instruções de instalação (Windows): [ToonCrafter-for-windows](https://github.com/sdbds/ToonCrafter-for-windows).
- [GitHub] Código fonte com instruções de instalação (Interface alternativa - ComfyUI): [ComfyUI-ToonCrafter](https://github.com/AIGODLIKE/ComfyUI-ToonCrafter).

#### Nuvem
- [Colab] Código para executar no Jupyter Notebook via Colab (Exige a versão Pro do Colab): [ToonCrafter_jupyter](https://colab.research.google.com/github/camenduru/ToonCrafter-jupyter/blob/main/ToonCrafter_jupyter.ipynb)
- [HuggingFace Spaces] Executar em nuvem (limite de execuções por tempo): [ToonCrafter: Generative Cartoon Interpolation](https://huggingface.co/spaces/Doubiiu/tooncrafter).
- [Replicate] Executar em nuvem (Pago por execução): [tooncrafter](https://replicate.com/fofr/tooncrafter).

---
### **Equipe de Desenvolvedores**

Os principais responsáveis pelo desenvolvimento incluem:

1. **Jinbo Xing**:
   - Pesquisador principal do projeto, com experiência em aprendizado de máquina aplicado à geração de conteúdo visual.
   - Trabalhou em colaboração com o Tencent AI Lab para integrar modelos de difusão no domínio de animações.

2. **Hanyuan Liu**:
   - Contribuiu para a adaptação dos modelos de difusão ao estilo visual único das animações.
   - É especializado em otimização de aprendizado profundo para aplicações gráficas.

3. **Menghan Xia**:
   - Trabalhou em métodos avançados de recuperação de detalhes e mecanismos de controle interativo, como o uso de esboços.

4. **Yong Zhang**:
   - Focado em aspectos de coerência temporal e eficiência computacional para processar animações com menor uso de memória.

5. **Xintao Wang** e **Ying Shan**:
   - Pesquisadores seniores do Tencent AI Lab, conhecidos por suas contribuições no campo de visão computacional e aprendizado profundo.

6. **Tien-Tsin Wong**:
   - Professor da CUHK, especializado em computação gráfica e renderização não-fotorealista.
   - Supervisou o projeto e garantiu que a solução atendesse aos padrões acadêmicos e de inovação.

---
### **Referências**

‌Doubiiu/ToonCrafter · Hugging Face. Disponível em: <https://huggingface.co/Doubiiu/ToonCrafter>. Acesso em: 1 dez. 2024.

DOUBIIU. GitHub - Doubiiu/ToonCrafter: [SIGGRAPH Asia 2024, Journal Track] ToonCrafter: Generative Cartoon Interpolation. Disponível em: <https://github.com/Doubiiu/ToonCrafter>. Acesso em: 1 dez. 2024.

XING, J. et al. ToonCrafter: Generative Cartoon Interpolation. Disponível em: <https://arxiv.org/abs/2405.17933>. Acesso em: 1 dez. 2024.

Presentation - SIGGRAPH Asia 2024. Disponível em: <https://asia.siggraph.org/2024/presentation/?id=papers_156&sess=sess135>. Acesso em: 1 dez. 2024.

ToonCrafter: Generative Cartoon Interpolation. Disponível em: <https://doubiiu.github.io/projects/ToonCrafter/>. Acesso em: 1 dez. 2024.