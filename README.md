# Autoencoder para Remoção de Ruído em Imagens

Este projeto implementa um Autoencoder Convolucional para remover ruídos de imagens. O modelo é treinado para aprender uma representação limpa de imagens a partir de suas versões ruidosas, sendo capaz de "denoise" novas imagens.

## Visão Geral do Projeto

O objetivo principal deste notebook é demonstrar a aplicação de um autoencoder para a tarefa de remoção de ruído (denoising). O processo envolve:

1.  **Preparação dos Dados:** Carregamento de um conjunto de imagens, redimensionamento e normalização.
2.  **Adição de Ruído:** Geração de ruído artificial (gaussiano e sal e pimenta) nas imagens para criar os dados de entrada ruidosos para o autoencoder.
3.  **Construção do Autoencoder:** Definição de uma arquitetura de rede neural convolucional para o autoencoder.
4.  **Treinamento:** Treinamento do autoencoder com pares de imagens (ruidosa, original) para que ele aprenda a reconstruir a imagem original a partir da versão ruidosa.
5.  **Avaliação e Visualização:** Geração de imagens denoised e visualização comparativa com as imagens originais e ruidosas.
6.  **Inferência em Imagens Novas:** Capacidade de carregar uma imagem externa e aplicar o processo de denoising.

## Como Executar o Projeto

Este projeto é projetado para ser executado no Google Colab.

### 1. Preparação no Google Colab

1.  **Faça Upload do Notebook:** Faça upload do arquivo `autoencoder.ipynb` para o seu Google Drive.
2.  **Abra no Colab:** Abra o notebook no Google Colab.
3.  **Monte o Google Drive:** A primeira etapa do notebook monta seu Google Drive para acessar arquivos. Você precisará permitir o acesso quando solicitado.
    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```
4.  **Prepare o Arquivo de Imagens:** O notebook espera um arquivo ZIP chamado `pasta2.zip` localizado em `/content/drive/MyDrive/gh/`. Este arquivo ZIP deve conter as imagens que serão usadas para treinar e testar o autoencoder. Certifique-se de que a estrutura de diretórios dentro do ZIP seja compatível com `IMG_DIR = '/content/imagens/pasta2'`. Por exemplo, se `pasta2.zip` contiver uma pasta `pasta2` com as imagens dentro.

### 2. Executando as Células

Execute as células do notebook sequencialmente.

* **Instalar Dependências:** A primeira célula instala as bibliotecas necessárias.
* **Extrair Arquivos:** A célula de extração irá descompactar suas imagens para o diretório `/content/imagens`.
* **Carregar Imagens:** Carrega as imagens do diretório especificado.
* **Adicionar Ruído e Treinar:** As células subsequentes preparam os dados, constroem e treinam o autoencoder.
* **Visualizar Resultados:** Células para exibir e salvar as imagens originais, ruidosas e denoised.
* **Baixar Resultados:** Uma célula permite baixar um arquivo ZIP contendo as imagens de resultado.
* **Inferência com Imagem Externa:** Há seções no final do notebook que permitem que você faça upload de uma nova imagem e veja o autoencoder aplicar o denoído nela.

### 3. Modificações Importantes

* **`zip_path`:** Ajuste o caminho para o seu arquivo ZIP de imagens na célula "Extrai os arquivos" se for diferente:
    ```python
    zip_path = '/content/drive/MyDrive/gh/pasta2.zip'
    ```
* **`IMG_SIZE`:** Você pode ajustar o tamanho das imagens na célula "Variáveis globais". Lembre-se que um tamanho maior pode exigir mais recursos de memória e tempo de treinamento.
    ```python
    IMG_SIZE = (96, 96) # Exemplo
    ```
* **Número de Imagens para Treino (`N`):** Para testes rápidos, o notebook limita o número de imagens a 3000. Você pode remover ou ajustar essa linha para usar mais imagens se desejar:
    ```python
    N = 3000
    if len(images) > N:
       images = images[:N]
    ```
* **Épocas de Treinamento (`epochs`):** O autoencoder é treinado por 5 épocas. Você pode aumentar este número para um melhor desempenho, mas levará mais tempo.
    ```python
    history = autoencoder.fit(
        x_train_noisy, x_train,
        epochs=5, # Altere este valor
        batch_size=8,
        shuffle=True,
        validation_data=(x_test_noisy, x_test)
    )
    ```

## Resultados Esperados

Após o treinamento e a execução do notebook, você verá:

* Gráficos comparando imagens originais, suas versões com ruído e as imagens denoised pelo autoencoder.
* Arquivos de imagem salvos no diretório `resultados/` no ambiente do Colab (e disponíveis para download via ZIP).
* A capacidade de testar o denoising em uma imagem personalizada que você fizer upload durante a execução do notebook.

## Dependências

As principais bibliotecas utilizadas neste projeto são:

* `tensorflow` e `keras` para a construção e treinamento da rede neural.
* `numpy` para manipulação de arrays.
* `Pillow` (PIL) e `scikit-image` para processamento de imagens e adição de ruído.
* `matplotlib` para visualização.
* `scikit-learn` para divisão dos dados de treino e teste.

As dependências são instaladas automaticamente na primeira célula do notebook:

```bash
!pip install pillow scikit-image matplotlib scikit-learn tensorflow
