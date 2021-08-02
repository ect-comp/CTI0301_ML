# MLP - Deep Learning 

Esse exemplo é uma desmonstração didática usando como base de dados Iris. 

![KNIME_keras_mlp](https://user-images.githubusercontent.com/19957124/127913292-5f976fa4-52bc-4b61-b1d4-ef64d41af115.png)

## Pré-processamento 

### File Reader 
* Base de dados Iris 
* Link para a base de dados: https://raw.githubusercontent.com/ect-info/ml/master/dados/Iris.csv 

### Rule Engine 
* Expressão para substituir o nome das classes por números inteiros 
```php
$Species$ = "Iris-setosa" => 0 
$Species$ = "Iris-virginica" => 1 
TRUE => 2
```
* Escolher a opção substituir a coluna "Species" 

### Create Collection Column 
* Incluir apenas a coluna "Species" 
* Escolher um nome para a nova columa, por exemplo "class_collection"

### Partitioning 
* Pode usar 75% 

### Normalizer 
* Incluir as colunas: "SepalLengthCm","SepalWidthCm", "PetalLengthCm" e "PetalWidthCm" 
* Escolher Z-Score 

### Normalizer (Apply)
* Deixar a configuração padrão 

## Montagem da Rede 

### Keras Input Layer
* Escolher 4 unidades 

### Keras Dense Layer 
* 8 Unidades 
* Função de ativação: ReLU

### Keras Dense Layer 
Este nó será usado como saída da rede 
* 3 unidades (uma para cada classe)
* Função de ativação: Softmax 

## Treinamento 

### Keras Network Learner 
Em dados de entrada:
* Incluir: "SepalLengthCm","SepalWidthCm", "PetalLengthCm" e "PetalWidthCm" 

Em dados de saída (_target_): 
* Incluir: class_collection 
* Escolher a função "_binary cross entropy_" como função de custo padrão  

Na aba de opções: 
* Épocas 80 
* Tamanho do lote (_batch_) de treinamento: 4 
* Algoritmo de treinamento, (otimizador): Adam  

## Avaliação 

### Keras Network Executor 
* Entradas: "SepalLengthCm","SepalWidthCm", "PetalLengthCm" e "PetalWidthCm" 
* Saídas: 
  * Adicionar a segunda camada, _Dense Layer_. 

### Rule Engine 
Regra para converter de números reais para uma categoria, valor inteiro. Em essência é encontrar o maior de três números e retornar a sua posição (0, 1 ou 2). Se por acaso aparece um valor igual nas saídas, a regra retorna o valor 3 (as linhas anteriores falharam). 
```php
$dense_2_0:0_0$ > $dense_2_0:0_1$ AND $dense_2_0:0_0$ > $dense_2_0:0_2$ => 0 
$dense_2_0:0_1$ > $dense_2_0:0_0$ AND $dense_2_0:0_1$ > $dense_2_0:0_2$ => 1 
$dense_2_0:0_2$ > $dense_2_0:0_0$ AND $dense_2_0:0_2$ > $dense_2_0:0_1$ => 2 
TRUE => 3
```

### Scorer 
* Primeira coluna: _Species_  
* Segunda coluna: *prediction_final*


![matriz_conf](https://user-images.githubusercontent.com/19957124/127913331-0decb48c-971a-4987-b42b-3c45af2d4690.png)
