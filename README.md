# **SomatiVEP_v01-2022**🔬 <!-- omit in toc -->
Pipeline para Anotação de Arquivo VCF utilizando o  Ensembl Variant Effect Predictor(VEP) version 105.0 via Google Colab

  ***SomatiVEP_v01** é de código aberto e está disponível no GitHub* 
  
* Introdução
  * Criar diretórios 
  * Instalar Programas 
  * Material Fornecido
  * Adicionar Arquivos 
* Aplicação 
  * Aplicar VEP para filtrar arquivo VCF
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
    - Referência do genoma que deseja estudar - no caso disponibilizamos para Homo Sapiens GRCh37 (Homo_sapiens_assembly19.fasta)
    
**Tempo de duração para rodar o pipeline: *~25-30 minutos***

> # **Preparação do Ambiente**

> ## *Criar diretórios*

  >> Crie uma conta no Google e acesse Google Colab 
  >> (disponível em: https://colab.research.google.com/#create=true)
  
  É necessário criar um espaço de diretórios para poder organizar seus arquivos input e output dentro do Google Colab. 
  
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
  Obs1.: Mova os diretórios para dentro do drive utilizando o comando `mv` + nomes_direitorios; \
  Obs2.: É possível você crie já as pastas necessárias dentro do seu Google Drive e quando você linkar com o ambiente de nuvem, terá já o ambiente para receber seus outputs. \
  Obs.3: Para confirmar que o diretório que será utilizado é o que queremos, podemos usar: `%%bash` + `pwd`
  
> ## *Instalar Programas*

**A. Instalando VEP ensembl-vep-105.0 - via GitHub**

Link: https://github.com/Ensembl/ensembl-vep/tags \
Neste link vai ter o repositório não apenas da versão 105.0 do VEP, mas de todas versões que já foram lançadas.

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

> ## *Material Fornecido*

**C. Arquivo Fasta e referência Homo Sapiens**

Agradecimento especial ao profissional **Renato Puga**👀 pelo fornecimento do material.

Disponível em: https://drive.google.com/drive/folders/1s_UInfwIbATc8qEw4pBbT5w-Tdb9P2MF

Utilizar seguinte arquivo:
- "homo_sapiens_refseq/Homo_sapiens_assembly19.fasta"

> ## *Adicionar Arquivos*

**D. Formato do Arquivo**

É possível subir arquivos no formato VCF que conectam o sequenciamento, independentemente da origem do sequenciamento. Arquivos compactados do tipo .gz também são aceitos.
Exemplo:
![image](https://user-images.githubusercontent.com/57289531/201490634-41b22301-cb77-4e8b-97cc-cb0ab3df168a.png)

```
from google.colab import files
Seu_Arquivo_VCF = files.upload()
```
![image](https://user-images.githubusercontent.com/57289531/201490082-5681592b-e363-4eba-b95c-89b884f597c8.png)

Obs.: Qualquer arquivo VCF que se encaixe no modelo acima que esteja na sua máquina, você pode utilizar.

**Verifivcação do VCF - executando o script `!zgrep -cv "#"` + caminho onde está seu vcf**

Exemplo:
```
!zgrep -cv "#" /content/drive/sua_Pasta/homo_sapiens_refseq/105_GRCh37/TESTE.filtered.vcf.gz 
```

> # **Aplicação**

**Aplicar VEP para filtrar arquivo VCF**

```
%%bash
./ensembl-vep-105.0/vep  \
  --fork 3 \
	-i /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz \
	-o /content/drive/Shareddrives/T4-2022/GuilhermeBueno/NOVEMBRO2022/WP312.filtered.vcf.tsv \
  --dir_cache /content/drive/Shareddrives/T4-2022/ \
  --fasta /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/Homo_sapiens_assembly19.fasta \
  --cache --offline --assembly GRCh37 --refseq  \
	--pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \
  --fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
  --individual all
```

1. Ativa o programa ensembl-vep-105.0;
2. Mandar ele rodar em 3 máquinas;
3. Colocar o input de onde está o arquivo VCF que deseja rodar no programa;
4. Diretório onde deseja salvar seu output;
5. Localizar onde está as informações necessárias para rodar
6. Configurações de filtragem e análise do arquivo VCF

*Tempo de Instalação: ~6-8 minuto --> 17.151 variantes (WP312.filtered.vcf.gz)*
