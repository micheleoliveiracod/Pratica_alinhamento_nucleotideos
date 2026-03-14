# Pipeline simples de alinhamento de sequências com BLAST, MAFFT e AliView
Este repositório documenta uma prática básica de bioinformática: criação de ambiente Conda, instalação de BLAST e MAFFT no Linux, download de sequências FASTA do NCBI, alinhamento no Linux e visualização do alinhamento no AliView no Windows.

## 1. Instalar Miniconda no Linux (via bash)
No terminal Linux:
criar pasta para o instalador
mkdir -p ~/miniconda3

baixar o instalador mais recente do Miniconda (Python 3, 64 bits)
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh

executar o instalador em modo silencioso
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3

remover o instalador
rm -f ~/miniconda3/miniconda.sh

inicializar o conda no bash
~/miniconda3/bin/conda init bash

reiniciar o shell (feche e abra o terminal, ou rode:)
source ~/.bashrc

## 2. Criar e ativar o ambiente Conda (bioinfo)
criar ambiente com nome bioinfo
conda create -n bioinfo python=3.11 -y

ativar o ambiente
conda activate bioinfo

Configurar canais para bioinformática (Bioconda)
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge

## 3. Instalar BLAST e MAFFT no ambiente
Com o ambiente bioinfo ativo:
instalar BLAST (ferramentas BLAST+ da NCBI)
conda install -c bioconda blast -y

instalar MAFFT (alinhamento múltiplo de sequências)
conda install -c bioconda mafft -y

Verificar versões:
blastn -version
mafft --version

## 4. Instalar AliView no Windows
No Windows, baixe e instale o AliView (visualizador/editor de alinhamentos).
Acesse o site oficial do AliView:
https://ormbunkar.se/aliview/
Baixe o instalador para Windows (.exe ou .msi).
Execute o instalador e siga os passos padrão (Next, Next, Finish).
Após instalar, abra o AliView para confirmar que está funcionando.

## 5. Baixar sequências FASTA do NCBI
Acesse o NCBI Nucleotide:
https://www.ncbi.nlm.nih.gov/nuccore/
Busque uma sequência de interesse (por exemplo, um gene associado ao Alzheimer / APOE ou outro gene).
Repita o processo em NCBI Virus para uma sequência viral:
Acesse: https://www.ncbi.nlm.nih.gov/labs/virus/
Busque um vírus (por exemplo Influenza H5N1 ou SARS-CoV-2).
Baixe em formato FASTA como seq_virus.fasta.
Copie os arquivos para seu ambiente Linux.

## ​6. Alinhamento par a par com BLAST (Linux)
Com o ambiente bioinfo ativo e os arquivos FASTA no diretório atual:

conda activate bioinfo

conferir arquivos:
ls *.fasta

6.1. Alinhamento par a par (Nucleotide e Vírus)

blastn -query Alzaimer_ApoE4.fasta \
       -subject Alzaimer_ApoE3.fasta \
       -outfmt 0 \
       -out alinhamento_Alzaimer_ApoE3+4_blast.fasta

less alinhamento_Alzaimer_ApoE34_blast.fasta

blastn -query Influenza_H5N1.fasta \
       -subject Influenza_H5N1.fasta \
       -outfmt 0 \
       -out alinhamento_Influenza_H5N1_blast.fasta

less alinhamento_Influenza_H5N1_blast.fasta

## 7. Alinhamento com MAFFT (Comparação entre sequências)

conferir
ls *.fasta
Alzaimer_ApoE3.fasta  Alzaimer_ApoE4.fasta

Junte as duas em um único arquivo multi‑FASTA:
cat Alzaimer_ApoE3.fasta Alzaimer_ApoE4.fasta > duas_sequencias.fasta

Rode o MAFFT neste arquivo multi‑FASTA:
mafft Alzaimer_ApoE3+4.fasta > alinhamento_Alzaimer_ApoE3+4_mafft.fasta

Ou, deixando o MAFFT escolher a melhor estratégia automaticamente:
mafft --auto Alzaimer_ApoE3+4.fasta > alinhamento_Alzaimer_ApoE3+4_mafft.fasta


## 8. Visualizar alinhamentos no AliView (Windows) - somente alinhamentos feitos com o MAFFT
Copie os arquivos de alinhamento (alinhamento_Alzaimer_ApoE3+4_mafft.fasta) do Linux para o Windows:

No Windows, abra o AliView.

Em File → Open alignment, selecione:

alinhamento_Alzaimer_ApoE3+4_mafft.fasta

Navegue pelas colunas de alinhamento, verifique identidade, gaps, etc.

Exemplo de visualização típica: duas sequências nas linhas, posições alinhadas por coluna, com cores por tipo de base/resíduo.

## 9. Sobre os metodos Blast e Mafft

BLAST – quando usar
Ferramenta de busca: você coloca uma sequência (query) e o BLAST procura sequências semelhantes em um banco (NCBI, por exemplo).

Responde perguntas como:

“Minha sequência parece com qual gene conhecido?”

“De que espécie pode ser essa sequência desconhecida?”

“Quão semelhante é essa sequência de Alzheimer a outra?”

Saída principal: alinhamentos locais par a par com estatísticas (identidade %, E‑value, bit score) para dizer se o match é forte ou fraco.

Use BLAST quando quiser identificar, comparar ou encontrar homólogos em bancos de dados.

MAFFT – quando usar
Ferramenta de alinhamento múltiplo de sequências: você fornece 2, 3, 10, 100+ sequências (do mesmo gene, por exemplo), e ele alinha tudo em colunas.

Responde perguntas como:

“Onde esse gene é conservado e onde ele varia entre espécies/variantes?”

“Quais posições do gene Alzheimer mudam mais?”

“Como preparar um alinhamento para árvore filogenética?”

Saída: um arquivo de alinhamento (FASTA/PHYLIP, etc.) que você vê em visores como AliView, analisando mutações, gaps e regiões conservadas.

Use MAFFT quando quiser comparar diretamente várias sequências alinhadas posição a posição (por exemplo, diferentes versões do mesmo gene de Alzheimer).



<div align="center">
  <img src="https://visitor-badge.laobi.icu/badge?page_id=micheleoliveiracod.Pratica_alinhamento_nucleotideos&"  />
</div>
