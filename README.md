# Processamento Vetorizado para Criptografia de Imagens Sensíveis

## Integrantes do Grupo

| Nome | RA |
|------|-----|
| Jhonatan Frossard | 200304 |
| João Victor Athayde Grilo | 210491 |
| Julio Cesar Bonow Manoel | 210375 |
| Rafael Henrique Ramos | 210432 |
| Rafael Rocha Leite | 222469 |
| Rickelme G Dias | 224276 |

## Sobre o Projeto

O projeto implementa um sistema de criptografia de imagens utilizando técnicas de processamento vetorizado com NumPy. O sistema combina operações XOR com matrizes QRNG (Quantum Random Number Generator) e mapas logísticos caóticos para embaralhar e criptografar imagens sensíveis, baseando-se e ligado ao TCC do Rafael Henrique, integrante de nosso grupo.

### Enquadrando ao Tema

**Tema Aplicado:** Processamento Vetorizado (NumPy/Pandas)

O projeto demonstra a aplicação prática de **processamento vetorizado** conforme solicitado nos requisitos da disciplina de Programação Distribuída. A implementação inclui:

- [x] **Processamento Vetorizado com NumPy**: Operações matriciais otimizadas para manipulação de imagens
- [x] **Comparação de Performance**: Análise comparativa entre implementações vetorizadas e não vetorizadas
- [x] **Aplicação Prática de TCC**: Sistema de criptografia de imagens com técnicas de computação quântica e caótica

## Objetivos

O foco principal implementar, com NumPy, o processamento vetorizado em um sistema de criptografia para imagens sensíveis, de modo a comparar a pipeline de processamento com e sem o sistema de vetorização.

### Objetivos Específicos

- Implementar operações criptográficas usando vetorização NumPy
- Comparar performance entre abordagens vetorizadas e não vetorizadas
- Demonstrar ganhos de eficiência no processamento de imagens
- Integrar técnicas de computação quântica (QRNG) e caótica (mapa logístico)

## Contextualização

Este projeto surgiu como um ponto de melhoria ao trabalho de conclusão de curso intitulado **"Computação Quântica e Criptografia Pós-Quântica Aplicadas à Segurança de Dados Sensíveis"**.

Nesse contexto, o TCC propõe implementar um sistema de proteção de imagens sensíveis que combina três módulos principais:

1. **Geradores Quânticos de Números Aleatórios (QRNG)**: Fonte de aleatoriedade verdadeira para operações criptográficas
2. **Sistema de cifragem baseado em operações de XOR e mapa caótico logístico**: Transformação e embaralhamento dos pixels da imagem
3. **Encapsulamento de chaves com o algoritmo pós-quântico CRYSTAL-Kyber 512**: Proteção das chaves criptográficas contra ataques de computadores quânticos

**Escopo deste projeto:** Este trabalho foca especificamente no **módulo 2** (cifragem com XOR e mapa logístico), implementando-o com processamento vetorizado para otimização de performance.

## Como Funciona

O sistema implementa um pipeline de criptografia em duas etapas principais:

### 1. **Operação XOR com QRNG**
A primeira camada de criptografia aplica a operação XOR bit a bit entre cada pixel da imagem e uma matriz de números aleatórios quânticos (QRNG).

**Funcionamento:**
- Carrega a imagem original e a matriz QRNG
- Realiza a operação `pixel_cifrado = pixel_original ⊕ qrng_value`
- Suporta imagens RGB (3 canais) e escala de cinza
- A versão vetorizada utiliza broadcasting do NumPy para processar todos os pixels simultaneamente

**Vantagem da vetorização:** Elimina laços duplos ou triplos (altura × largura × canais), processando toda a imagem em uma única operação matricial.

### 2. **Permutação com Mapa Logístico**
A segunda camada embaralha a posição dos pixels usando uma sequência caótica.

**Funcionamento:**
- Gera uma sequência caótica usando o mapa logístico: `x[n+1] = r × x[n] × (1 - x[n])`
- Utiliza parâmetros: `r = 3.999` e `x₀ = 0.98892455322743` (regime caótico)
- Ordena os índices dos pixels com base nos valores caóticos
- Reposiciona os pixels seguindo essa ordem
- A versão vetorizada usa `np.argsort()` e indexação avançada para reordenar eficientemente

**Vantagem da vetorização:** A indexação avançada do NumPy permite permutar milhões de pixels em milissegundos, comparado a segundos com laços explícitos.

### Vantagens do Processamento Vetorizado

O projeto demonstra significativos ganhos de performance ao utilizar operações vetorizadas:

![Tabela de Ganhos](https://github.com/user-attachments/assets/6528fb79-6d82-4b79-91fb-794fa2cb34e7)

*Valores aproximados para imagem 1280×1280 pixels RGB*

**Razões técnicas para o ganho:**
- Eliminação de overhead de laços Python
- Melhor uso de cache e instruções SIMD
- Operações implementadas em C (NumPy)
- Processamento paralelo em nível de hardware

## Recursos Utilizados

### Bibliotecas Principais

```
numpy>=1.24.0          # Processamento vetorizado e operações matriciais
opencv-python>=4.8.0   # Leitura e manipulação de imagens
matplotlib>=3.7.0      # Visualização de resultados
```

### Arquivo requirements.txt

```txt
numpy>=1.24.0
opencv-python>=4.8.0
matplotlib>=3.7.0
```

## 🛠️ Instalação e Configuração

### Opção 1: Ambiente Conda (Recomendado)

1. **Criar ambiente virtual:**
```bash
conda create -n vector python=3.12
```

2. **Ativar o ambiente:**
```bash
conda activate vector
```

3. **Instalar dependências:**
```bash
pip install -r requirements.txt
```

### Opção 2: Ambiente Python Virtual (venv)

1. **Criar ambiente virtual:**
```bash
python -m venv vector_env
```

2. **Ativar o ambiente:**
   - **Linux/Mac:**
   ```bash
   source vector_env/bin/activate
   ```
   - **Windows:**
   ```bash
   vector_env\Scripts\activate
   ```

3. **Instalar dependências:**
```bash
pip install -r requirements.txt
```

### Estrutura de Diretórios

Antes de executar, certifique-se de que a estrutura de diretórios está correta:

```
projeto/
├── main.py
├── requirements.txt
├── README.md
├── inputs_images/
│   └── image_input_1280.jpg    # Sua imagem de entrada
└── matrices/
    └── matriz_qrng_1280_001.png # Matriz QRNG
```

## Como Executar

### 1. Preparar os Arquivos de Entrada

- Coloque sua imagem em `inputs_images/image_input_1280.jpg`
- Coloque a matriz QRNG em `matrices/matriz_qrng_1280_001.png`
- **Importante:** A imagem e a matriz QRNG devem ter as mesmas dimensões

### 2. Configurar Parâmetros (Opcional)

Edite as constantes no início do arquivo `main.py`:

```python
CAMINHO_IMAGEM = "inputs_images/image_input_1280.jpg"  # Caminho da imagem de entrada
CAMINHO_QRNG   = "matrices/matriz_qrng_1280_001.png"   # Caminho da matriz QRNG
TAMANHO_CORTE = None  # None = imagem completa, ou especifique tamanho (ex: 512)
```

### 3. Executar o Programa

```bash
python main.py
```


## Implementações

### Implementações Vetorizadas (Otimizadas)

#### `aplicar_xor_com_qrng_vectorizado(imagem, matriz_qrng)`
- Aplica XOR usando operações matriciais NumPy
- Utiliza broadcasting para processar todos os canais simultaneamente
- Suporta imagens RGB e escala de cinza

#### `logistic_map_vectorizado(size, r=3.999, x0=0.98892455322743)`
- Gera sequência caótica usando array pré-alocado
- Parâmetros otimizados para regime caótico máximo

#### `aplicar_lm_vectorizado(imagem)`
- Permuta pixels usando indexação avançada do NumPy
- Usa `np.argsort()` para ordenação eficiente
- Reshape inteligente para preservar estrutura da imagem

### Implementações Não Vetorizadas (Baseline)

#### `aplicar_xor_com_qrng_nao_vectorizado(imagem, matriz_qrng)`
- XOR com laços explícitos triplos (altura × largura × canais)
- Serve como baseline para comparação de performance

#### `logistic_map_nao_vectorizado(size, r=3.999, x0=0.98892455322743)`
- Geração sequencial usando lista + append
- Demonstra overhead do Python

#### `aplicar_lm_nao_vectorizado(imagem)`
- Permutação com laços aninhados
- Acesso individual a cada pixel

### Funções Auxiliares

#### `cortar_centro(imagem, tamanho)`
- Recorta região central quadrada da imagem
- Útil para testes com diferentes resoluções
- Retorna `None` se a imagem for menor que o corte

#### `mostrar_imagem(imagem, titulo)`
- Visualiza uma imagem usando matplotlib
- Converte automaticamente de BGR (OpenCV) para RGB (matplotlib)

#### `mostrar_lado_a_lado(img1, img2, titulo1, titulo2)`
- Compara duas imagens lado a lado
- Facilita análise visual das diferenças

## 📈 Análise de Performance

### Metodologia de Medição

O projeto utiliza a biblioteca `time` do Python para medir com precisão:
- Tempo de execução do XOR vetorizado vs. não vetorizado
- Tempo de execução do mapa logístico vetorizado vs. não vetorizado
- Tempo total do pipeline completo

### Fatores que Influenciam a Performance

1. **Tamanho da Imagem**: Imagens maiores amplificam a diferença de performance
2. **Número de Canais**: RGB (3 canais) vs. escala de cinza (1 canal)
3. **Hardware**: CPU com suporte a instruções SIMD (AVX, AVX2) tem maior ganho
4. **Memória Cache**: Imagens que cabem em cache L3 têm melhor performance

## Conceitos de Programação Distribuída Aplicados

### Processamento Vetorizado
- **Paralelização implícita**: Instruções SIMD (Single Instruction, Multiple Data)
- **Throughput**: Múltiplos dados processados por ciclo de clock
- **Escalabilidade**: Código preparado para GPUs (CuPy) e clusters (Dask)

### Otimização de Memória
- **Acesso contíguo**: Melhor aproveitamento de cache
- **Views vs. Cópias**: NumPy evita cópias desnecessárias
- **Broadcast**: Reduz uso de memória ao evitar expansão explícita

### Preparação para Distribuição
- **Independência de dados**: Cada pixel pode ser processado independentemente
- **Divisão de trabalho**: Imagem pode ser dividida em blocos
- **Potencial para MPI/Dask**: Estrutura facilita paralelização futura
---

**Desenvolvido com 💻 e ☕ pela equipe de Programação Distribuída 2024/2**
