# **SomatiVEP_v01-2022** <!-- omit in toc -->
Pipeline para Anotação de Arquivo VCF utilizando o  Ensembl Variant Effect Predictor(VEP) version 105.0 via Google Colab

  ***SomatiVEP_v01** é de código aberto e está disponível no GitHub* 
  
* Introdução
  * Criar diretórios 
  * Instalar Programas 
  * Adicionar Arquivos 
* Como utilizar?
  * Argumentos
  * Exemplo
* Contato    

> # **Introdução**

  Sejam bem-vindo, bioinformatas! \
  SomatiVEP-v01 é uma script utilizado para você analisar os arquivos no formato VCF e filtrar as variantes de interesse de uma amostra de herança somática. Código foi criado de maneira que possa utilizar via Google Colab sem grandes dificuldades. 

SomatiVEP_v01 precisa ter as seguintes requisitos: 

    - Linguagens utilizadas: Bash e Python (version 3.11.10); 
    - Biblioteca Python: pandas; 
    - Boa conexão com internet; 
    - Conta no Google Colab; 
    - Espaço de armazenamento no Google Colab.
    - Referência do genoma que deseja estudar - no caso disponibilizamos para Homo Sapiens GRCh37
    - Arquivo fasta - no caso disnponibilizamos para Homo_sapiens_assembly19

Tempo de duração para rodar o pipeline: *~25-30 minutos*

> # **Preparação do Ambiente**

> ## *Criar diretórios*

  >> Crie uma conta no Google e acesse Google Colab 
  >> (disponível em: https://colab.research.google.com/#create=true)
  
  É necessário criar um espaço de diretórios para pode organizar seus arquivos input e output dentro do Google Colab. \
  Crie uma linha do código - Para linkar com ambiente nuvem:
  ```
  from google.colab import drive
  drive.mount('/content/drive')
  ```
  Crie os diretórios por linha do código:
  ```
  mkdir vep
  mkdir homoSapiens_refSeq
  mkdir analise
  ```
  Obs1.: Mova os diretórios para dentro do drive utilizando o comando 'mv' + nomes_direitorios;
  Obs2.: É possível você crie já as pastas necessárias dentro do seu google drive e quando você linkar com o ambiente de nuvem, terá já o ambiente para receber seus outputs.
  
  
> ## *Instalar Programas*

**A. Instalando VEP ensembl-vep-105.0 - via GitHub**

Link: https://github.com/Ensembl/ensembl-vep/tags \
Neste link vai ter o repositório não apenas da versão 105.0 do VEP, mas todas versões que desejar.

Use a linha de código para instalação:

```
%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE
```

1. Instalar as dependências;
2. Download da release ensembl-vep 105.0 no GitHub;
3. Descompactar o arquivo .tar.gz;
4. Entrar dentro do diretório;
5. Rodar o Script INSTALL.PL com a opção -NO_UPDATE (para não perguntar se quero a versão mais nova).

Obs.: --NO_UPDATE é a função para não atualizar para versões mais novas.

*Tempo de Instalação: ~1 minuto*

**Verifivcação do VEP - executando o script `./vep`**

1. Entrei dentro do diretório ensembl-vep-105.0
2. Executei o script `./vep`

```
%%bash
cd ensembl-vep-105.0/
./vep
```

**B. Instalando biblioteca Pandas**

```
!pip install pandas
```
*Tempo de Instalação: ~30 segundos*

> ## *Adicionar Arquivos*

**C. Formato do Arquivo**

É possível subir arquivos no formato VCF que conectam o sequenciamento, independentemente da origem do sequenciamento. Arquivos compactados do tipo .gz também são aceitos.
Exemplo:
![image](https://user-images.githubusercontent.com/57289531/201490634-41b22301-cb77-4e8b-97cc-cb0ab3df168a.png)

```
from google.colab import files
Seu_Arquivo_VCF = files.upload()
```
![image](https://user-images.githubusercontent.com/57289531/201490082-5681592b-e363-4eba-b95c-89b884f597c8.png)
Obs.: Qualquer arquivo VCF que se encaixe no modelo acima que esteja na sua máquina, você pode utilizar.
