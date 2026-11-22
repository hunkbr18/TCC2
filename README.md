# Reconhecimento de padrões visuais de grãos de arroz usando redes neurais implementadas em VHDL

Repositório referente ao Trabalho de Conclusão de Curso desenvolvido no curso de Engenharia Eletrônica da Universidade Tecnológica Federal do Paraná (UTFPR), Campus Campo Mourão.

O trabalho apresenta o desenvolvimento de uma Rede Neural Artificial (RNA) destinada à classificação de imagens de grãos de arroz em duas classes — grãos inteiros e quebrados — seguido da implementação da arquitetura treinada em linguagem VHDL.

## Organização do repositório

Este arquivo `README.md` está localizado na raiz do repositório. Os demais arquivos estão organizados em quatro diretórios principais:

```text
TCC2/
├── imagens_originais/
│   ├── IMG_20260911_154655656.jpg
│   ├── IMG_20260911_160514406.jpg
│   └── ...
│
├── matlab/
│   ├── Varredura_pixel_a_pixel_03.mlx
│   └── RNA_TCC7.mlx
│
├── dataset/
│   └── dataset_qualidade_gray.rar
│
├── vhdl/
│   ├── TCC teste RNA01.rar
│   ├── TCC teste RNA02.rar
│   ├── TCC teste RNA03.rar
│   ├── TCC teste RNA04.rar
│   ├── TCC teste RNA05.rar
│   └── TCC_final_RNA01.rar
│
└── README.md
```

### `imagens_originais/`

Diretório contendo as 45 fotografias originais utilizadas como entrada para o algoritmo de processamento de imagens.

Cada fotografia contém múltiplos grãos de arroz distribuídos sobre o plano de fundo utilizado durante a aquisição.

[Acessar diretório de imagens originais](./imagens_originais/)

### `matlab/`

Diretório contendo os códigos desenvolvidos no MATLAB®.

- `Varredura_pixel_a_pixel_03.mlx`: código relacionado ao processamento das fotografias, segmentação, identificação dos grãos, análise das propriedades geométricas, classificação inicial, recorte e geração das amostras individuais.
- `RNA_TCC7.mlx`: código utilizado para o desenvolvimento, treinamento e avaliação da Rede Neural Artificial, além da conversão e exportação dos parâmetros empregados posteriormente na implementação em VHDL.

[Acessar diretório MATLAB](./matlab/)

### `dataset/`

Diretório contendo o arquivo:

`dataset_qualidade_gray.rar`

O arquivo reúne as amostras obtidas após o processamento das fotografias. As imagens foram convertidas para escala de cinza e padronizadas em 20 × 20 pixels antes de serem utilizadas no desenvolvimento da RNA.

[Acessar diretório do dataset](./dataset/)

### `vhdl/`

Diretório contendo os projetos utilizados durante o desenvolvimento incremental da arquitetura em VHDL.

Os arquivos `TCC teste RNA01.rar` a `TCC teste RNA05.rar` correspondem às diferentes etapas de desenvolvimento e validação dos componentes da rede neural.

O arquivo:

`TCC_final_RNA01.rar`

corresponde ao projeto final utilizado nas etapas de simulação funcional, síntese e análise temporal apresentadas no Trabalho de Conclusão de Curso.

[Acessar diretório VHDL](./vhdl/)

## Visão geral do desenvolvimento

O desenvolvimento do projeto foi realizado nas seguintes etapas:

1. aquisição das fotografias dos grãos de arroz;
2. processamento e segmentação das imagens no MATLAB®;
3. identificação e recorte individual dos grãos;
4. geração das amostras padronizadas em 20 × 20 pixels;
5. treinamento e avaliação de uma rede neural do tipo Perceptron de Múltiplas Camadas (MLP);
6. conversão dos parâmetros da rede para representação em ponto fixo;
7. desenvolvimento incremental da arquitetura em VHDL;
8. simulação funcional no ModelSim-Altera;
9. síntese da arquitetura no Quartus Prime Lite Edition®;
10. análise da utilização de recursos e temporização da arquitetura sintetizada.

## Conjunto de dados

Foram utilizadas 45 fotografias originais de grãos de arroz.

Após o processamento das imagens, foram obtidas 10.151 amostras individuais padronizadas em imagens de 20 × 20 pixels.

| Classe | Quantidade de amostras |
| --- | ---: |
| `arroz_tipo1` | 5.084 |
| `arroz_tipo3_4` | 5.067 |
| **Total** | **10.151** |

As nomenclaturas `arroz_tipo1` e `arroz_tipo3_4` são identificadores computacionais utilizados no projeto e correspondem, respectivamente, às classes de grãos inteiros e quebrados. Esses identificadores não representam diretamente os tipos oficiais de classificação estabelecidos pelo MAPA.

A divisão entre o conjunto de desenvolvimento e o conjunto externo de teste foi realizada de acordo com a fotografia de origem das amostras:

| Conjunto | Fotografias | Amostras |
| --- | ---: | ---: |
| Desenvolvimento | 31 | 7.106 |
| Teste externo | 14 | 3.045 |

Todos os recortes provenientes de uma mesma fotografia foram mantidos no mesmo conjunto, reduzindo a possibilidade de compartilhamento de características entre desenvolvimento e teste externo.

## Arquitetura da Rede Neural Artificial

Foi utilizada uma rede do tipo Perceptron de Múltiplas Camadas (MLP) com arquitetura 400–4–2:

- 400 entradas, correspondentes aos pixels de uma imagem de 20 × 20 pixels;
- quatro neurônios na camada oculta;
- dois neurônios na camada de saída;
- função tangente sigmoide na camada oculta.

No conjunto externo de teste, composto por 3.045 amostras, a rede apresentou acurácia aproximada de 97%.

## Representação em ponto fixo

Os parâmetros obtidos durante o treinamento no MATLAB® foram convertidos para uma representação em ponto fixo com:

- 20 bits de largura total;
- 12 bits fracionários;
- fator de escala igual a 4096.

Os parâmetros convertidos foram posteriormente utilizados na implementação da arquitetura em VHDL.

## Desenvolvimento em VHDL

O desenvolvimento da arquitetura em VHDL foi realizado de maneira incremental.

As principais etapas envolveram:

1. validação do fluxo de dados de um neurônio;
2. implementação de um neurônio com operações de multiplicação, acumulação, bias e função de ativação;
3. integração de múltiplos neurônios em uma camada;
4. desenvolvimento de uma rede MLP preliminar;
5. implementação da arquitetura final 400–4–2 destinada à classificação das imagens dos grãos de arroz.

Os diferentes estágios do desenvolvimento estão disponibilizados no diretório [`vhdl/`](./vhdl/).

## Simulação e síntese

A validação funcional da arquitetura foi realizada no ModelSim-Altera.

A síntese da implementação final foi realizada no Quartus Prime Lite Edition® 17.1 para o dispositivo:

`Cyclone V 5CSEBA6U23I7`

A arquitetura final utilizou aproximadamente:

- 3% dos ALMs disponíveis;
- 5% dos blocos DSP disponíveis;
- 473 registradores;
- 6 blocos DSP;
- 0 bits de memória em bloco.

A análise por entidade mostrou que os seis blocos DSP estão distribuídos entre os seis neurônios da arquitetura:

- quatro DSPs na camada oculta;
- dois DSPs na camada de saída.

## Análise temporal

Para a análise temporal foi utilizada uma frequência nominal de operação de 40 MHz, correspondente a um período de clock de 25 ns.

As análises realizadas no TimeQuest Timing Analyzer apresentaram valores positivos de setup slack e hold slack nas condições avaliadas.

A frequência máxima estimada mais restritiva entre os modelos Slow analisados foi de:

**46,44 MHz**

A arquitetura apresentou uma latência aproximada de 406 ciclos de clock entre o início do fornecimento da amostra e a disponibilização da classificação.

Considerando a frequência nominal de 40 MHz, essa quantidade de ciclos corresponde a aproximadamente:

**10,15 µs por amostra**

## Ferramentas utilizadas

| Ferramenta | Utilização |
| --- | --- |
| MATLAB® | Processamento das imagens, geração dos conjuntos de dados, treinamento e avaliação da RNA e preparação dos parâmetros para VHDL |
| VHDL | Descrição dos componentes e da arquitetura digital da RNA |
| ModelSim-Altera | Simulação funcional dos componentes e da arquitetura final |
| Quartus Prime Lite Edition® 17.1 | Compilação, síntese, análise de recursos e análise temporal |

## Observações

A acurácia aproximada de 97% apresentada no trabalho corresponde à avaliação realizada no MATLAB® utilizando o conjunto externo completo de teste.

As simulações realizadas no ModelSim-Altera tiveram como objetivo validar funcionalmente a arquitetura em VHDL e não correspondem a uma avaliação da acurácia completa da implementação em hardware.

Os rótulos utilizados no conjunto de dados foram obtidos a partir do procedimento geométrico desenvolvido durante o processamento das imagens e não correspondem a uma classificação independente realizada por especialista.

A arquitetura foi validada por simulação funcional e síntese, não tendo sido realizada sua implementação e validação experimental em uma placa FPGA física.

## Autor

Héricles Roberto Schepanski Petenusso  
Engenharia Eletrônica  
Universidade Tecnológica Federal do Paraná – Campus Campo Mourão  
2026
