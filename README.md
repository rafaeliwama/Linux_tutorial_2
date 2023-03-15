# Tutorial Linux 2


Antes seguir para os próximos passos, vamos fazer mais alguns exercícios com o grep.

É possível executar o comando 'grep -E' simplesmente executando o comando 'egrep'. Desta forma, todos os exemplos utilizarão a forma 'egrep'.


## Blast. Uma ferramenta para identificar sequências homólogas

Da mesma forma que nós podemos estabelecer relações de homologia entre estruturas morfológicas, é possível estabelecer relações de homologia entre sequências. Por exemplo, a hemoglobina de humanos é homóloga à hemoglobina de chipanzés. O conceito de homologia em moléculas, no entando, é um pouco mais complexo. Foge do objetivo deste tutorial abordar este conceito, mas nós um uso importante do conceito de homologia entre sequências é a identificação de sequências.

Por exemplo, digamos que você tenha um arquivo fasta, com várias proteínas que são produzidas por uma espécie animal. Você não sabe quais são estas protéinas, mas você tem a sequência. Você pode utilizar o conceito de homologia para identificar estas proteínas e tentar entender possíveis funções destas proteínas.

O conceito de homologia, prediz que proteínas homólogas serão mais similares entre si, do que proteínas não-homólogas. Desta forma, é possível utilizar algorítmos que calculam se a similaridade entre duas sequências é maior do que o esperado por processos aleatórios. O processo é mais complexo do que isso, claro. Mas a linha de raciocínio é esta.

O Blast é um destes programas que são utilizados para estabelecer uma relação de homologia entre sequências. Mais tarde eu vou explicar melhor como o blast funciona, mas no geral ele estabelece relações de homologia recentes entre sequências de nucleotídeos e proteínas.

Se você quiser entender melhor sobre o blast, consulte o manual do software em: https://www.ncbi.nlm.nih.gov/books/NBK279690/

Hoje, nós vamos instalar o blast, identificar sequências que são possíveis anticoagulantes utilizando como base aquelas sequências que nós isolamos no tutorial passado, e utilizar alguns comandos para analisar os resultados.


## Instalação

Uma das atividades que mais consome o tempo do bioinformata é instalar programas que são utilizados nas análises. Alguns softwares possuem uma instalação bem trabalhosa, mas o blast não é um desses. Exatamente por isso, a primeira coisa que a gente vai instalar é ele.

Caso você esteja utilizando uma distribuição do linux no Windows 10, não se esqueça de se dirigir ao diretório '/home'

``` 
cd ~
``` 

faco o download do diretório que contém os arquivos do Blast+ com 'wget'.

```
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.10.0/ncbi-blast-2.10.0+-x64-linux.tar.gz
``` 
Com wget você pode fazer o download the arquivos apenas com o endereço em que o arquivo está hospedado.

Analise o nome do arquivo que você acabou de baixar. Hint: use o comando ls para listar os arquivos e diretórios.

Repare que o final do arquivo acaba com a sufixo .tar.gz
Arquivos .gz estão comprimidos e arquivos .tar são como diretórios que contém mais de um arquivo. Contudo este formato é especializado na distribuição de arquivos e não na execução e armazenamento.

Para descomprimir e converter arquivos .gz e .tar, utilize o comando 'tar'.

```
tar zxvpf ncbi-blast-2.10.0+-x64-linux.tar.gz
```

Lembre-se que 'zxvpf' são as opções que especificam a função de 'tar'.


Agora, inspecione o contéudo da pasta 'rmblast-2.13.0' utilizando os comandos 'cd' e 'ls'.

```
(base) rafael@malaco:~$ cd rmblast-2.13.0/
(base) rafael@malaco:~/rmblast-2.13.0$ ls
bin  LICENSE.rmblast  README.blast  README.rmblast
```

Perceba que há alguns arquivos com o sufixo '.rmblast' e '.blast'. Estes arquivos contém explicações sobre os softwares pertencentes ao 'Blast+ suite'. Dentro desse diretório, o diretório bin é o mais importante deste programa. Ela contém os arquivos binários que estão associados ao 'Blast+'. 

**Atividade:** inspecione o conteúdo do diretório 'bin'.

Estes arquivos binários são os arquivos executáveis do 'Blast+'. Para você executar estes arquivos, você terá que mudar de diretório atual para o diretório bin e indicar para o prompt que você deseja executar o arquivo binário no diretório em que ele está. Para isso utilize os seguintes comandos:

```
cd bin
./blastp
```

Onde:
bin - diretório que contém os arquivos binários.
./ - indica que você deseja executar o arquivo binário no diretório atual ou no path que você indicar.
blastp - é o software que você deseja executar. Neste caso, o blastp é a versão do blast que aceita sequências de proteína tanto como input, tanto como em sua base de dados.

Como resultado desses comandos, você verá um erro. Isso acontece porque nós utilizados a sintaxe correta do 'blastp'. Mas antes de utilizar-mos o blastṕ corretamente, nós temos que terminar de instalar o programa corretamente.

Assim como os comandos 'cd', 'mkdir', 'cat' ou 'grep' que você digita no terminal, estes programas podem ser chamados da mesma forma. Mas para isso nós precisamos adicionar estes arquivos binários no PATH. Aqui é importante que você diferencie o path e o PATH.


Enquanto o path é o caminho exato de um arquivo ou diretório. Já o PATH especifica o caminho de um diretório em que se encontra os arquivos binário para executá-los. Basicamente, quando você digita o comando 'cat' no terminal, o computador procura em uma série de diretório um arquivo binário com o nome 'cat' e o executa. O que nós queremos fazer é mover os arquivos binários do 'Blast+' para um diretório já especificado no PATH ou adicionar o diretório bin no seu PATH.

Hoje nós vamos fazer o primeiro.

Verifique quais diretórios estão no seu PATH
```
(base) rafael@malaco:~/rmblast-2.13.0$ echo $PATH
/home/rafael/anaconda3/bin:/home/rafael/anaconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```
Note que cada diretório especificado está separado por ':'. Como mostra a figura abaixo:
![Screenshot from 2023-03-14 15-21-21](https://user-images.githubusercontent.com/46658489/225094647-b12df718-ae44-4e68-a2ef-28d0e5a63bdb.png)


Nós vamos adicionar os arquivos da pasta '/rmblast-2.13.0/bin' no PATH, para isso você precisa editar um arquivo oculto que especifica quais diretórios estão no seu PATH. O arquivo se chama '~/.profile'.

**Atividade:** qual o significado de cada item no nome do arquivo '~/.profile'?


Nós vamos copiar a linha abaixo no final do arquivo '~/.profile', salvar e fechar o arquivo. Nós utilizaremos o nano!

Linha para ser copiada:
```
export PATH=$PATH:/home/rafael/rmblast-2.13.0/bin
```

Comandos para copiar:
1. copie a linha acima (como se você fosse copiar e colar algo)
2. digite no terminal 
```
nano ~/.bashrc
```
3. com o botão direito do mouse, cole o conteúdo copiado no final do arquivo.
4. Salve o arquivo seguindo as instruções do nano. (Ctr+o e Ctr+x)

5. use 'source ~/.bashrc' para carregar o novo PATH.
```
source ~/.bashrc
```

6. Teste se blastp está no seu PATH
```
blastp
```

## Base de dados do blast

Para rodar o blast, nós precisamos de uma base de dados para comparar com as sequências que nṍs queremos identificar. Nós vamos utilizar o arquivo fasta que contém as proteínas anticoagulantes que nós utilizamos no tutorial anterior

Para isso, dirija-se para o diretório que contém o arquivo 'anti_dec2016.fasta', que deve estar dentro do diretório 'Linux_tutorial'.

Caso você não o encontre, crie um diretório novo, e baixe o arquivo utilizando o comando 'git'.

```
git clone https://github.com/rafaeliwama/Linux_tutorial.git
```

Agora, utilize o comando makeblastdb para criar uma base de dados do blast utilizando o arquivo 'anti_dec2016.fasta'.

```
makeblastdb -in anti_dec2016.fasta -dbtype prot -out anti_dec2016
```
Onde:
1. makeblastdb - software utilizado para criar a base de dados

2. -in antidec2016.fasta - '-in' é opção que especifica o arquivo utilizado para criar a base

3. -dbtype prot - '-dbtype' é a opção que especifica o type de sequência da base e 'prot' indica que são sequências de proteína.

4. -out antidec2016 - '-out' é a opção que especifica a base do nome da base nos arquivos do output e 'antidec2016' é a base do nome utilizada.


Utilize o comando 'ls' para inspecionar o quais arquivos foram criados.

```
(base) Rafaels-MacBook-Pro:Linux_tutorial rafael$ ls
README.md		anti_dec2016.phr	tested_anticogs.txt
anti_dec2016		anti_dec2016.pin
anti_dec2016.fasta	anti_dec2016.psq
```

Veja que o 'makeblastdb' criou vários arquivos com o prefixo 'anti_dec2016'. Estes arquivos compõe a base de dados utilizada.


Agora, nós vamos utilizar o blast para identificar as sequências contidas em três transcriptomas de sanguessugas. Então, primeiro nós precisamos baixar os transcriptomas, que para facilitar eu coloquei aqui no GitHub.


Passos:
1. Para baixar, clone este repositório no seu 'home' directory

```
git clone https://github.com/rafaeliwama/Linux_tutorial_2.git
```

2. Vá para a pasta '~/Linux_tutorial_2/transcriptomes_tutorial'

3. faça a descompmressão dos arquivos contidos na pasta com 'gunzip' e inspecione os arquivos com 'head' e 'tail'.

**Pergunta:** Qual o tipo de sequência contido nos arquivos fasta que contém os transcriptomas de sanguessugas?


As sequências contidas nesse arquivo estão identificadas pela ordem em que elas foram montadas pelo softaware trinity. Eu vou explicar melhor o que esse programa faz, mas basicamente ele junta sequencias pequenas, de 100-150 nts em sequencias que representam sequencias de RNA.

Agora, nós vamos utilizar o software 'blastx' para anotar estas sequências.


## utilizando 'for' loops

Para você descomprimir os transcriptomas, você preciosu digitar três vezes o comando 'gunzip' e o nome do arquivo. Esta estratégia é inviável quando nós precisamos utilizar o mesmo comando para dezenas, centenas ou milhares de arquivos ou items. Nós podemos automatizar esse processo utilizando uma sintaxe chamada de 'for' loops.

'for' loops repetem a mesma sequência de comandos diversas vezes em um subset dos seus arquivos ou items.

Por exemplo, para executar o comando blastx você teria que executar os três comandos seguintes:

```
blastx -db anti_dec2016 -max_target_seqs 1 -evalue 1e-5 -outfmt 6 -query GBRF01.1.fsa_nt -out GBRF01.1.fsa_nt.blast.txt
blastx -db anti_dec2016 -max_target_seqs 1 -evalue 1e-5 -outfmt 6 -query GBRF01.1.fsa_nt -out GIVY01.1.fsa_nt.blast.txt
blastx -db anti_dec2016 -max_target_seqs 1 -evalue 1e-5 -outfmt 6 -query GBRF01.1.fsa_nt -out GIWB01.1.fsa_nt.blast.txt
```

Isso funcionaria, mas há uma forma melhor e mais rápida! Os 'for' loops.

For loops utilizam a seguinte sintaxe

'for VARIABLE in LOCATION; 
do COMMAND; 
do COMMAND; 
do COMMAND; 
done'

Onde:

VARIABLE - É um local de armazenamento abastrato. Você pode armazenar qualquer tipo de informação em uma variável. Um número. Uma string. Uma lista. Etc. Se você utilizar a sintaxe do for loop, a variável será criada automaticamente utilizando a palavra seguinte ao 'for'.
do COMMAND - São os commandos que você quer executar. Você pode executar quantos comandos quiser em sequência.
done - indica o final do loop.

lembro de utilizar o ';'!

Observe o seguinte loop:

```
for file in *.fsa_nt; 
do blastx -db /LOCAL/DO/SEU/BLAST/DABASE/anti_dec2016 -max_target_seqs 1 -evalue 1e-5 -outfmt 6 -query $file -out $file.blast.txt;
done
```
**Observe que você deve inserir o local da sua base de dados do blast que você criou acima!**

Esta análise deve demorar alguns minutos.

Vamos tentar entender o loop!

**A primeira linha leva:**

'for file in *.fsa_nt'

file - é a variavel que o loop vai criar. Esta variável só existirá enquanto a etapa que o loop está estiver ativa.
*.fsa_nt - o '*' indica 'qualquer string' e como este símbulo é seguido por 'fsa_nt', isso significa: qualquer string, seguido de '.fsa_nt'.

Na prática, o loop vai armazenar o nome dos arquivos terminados em '.fsa_nt' na variável 'file', executar os comandos dados para todos os arquivos que segue o padrão dado na primeira linha.

**A segunda linha:**

'do blastx -db /LOCAL/DO/SEU/BLAST/DABASE/anti_dec2016 -max_target_seqs 1 -evalue 1e-5 -outfmt 6 -query $file -out $file.blast.txt;'

'blastx' - é o comando

'-db /LOCAL/DO/SEU/BLAST/DABASE/anti_dec2016' - é o local da sua base de dados do blast

'-max_target_seqs 1 -evalue 1e-5 -outfmt 6' - são algumas opções do blast q eu vou ensinar mais tarde

'-query $file' - a opção '-query' é a opção que espeficifica o arquivo que vai ser utilizado como input. No nosso caso, o transcriptoma. Perceba que aqui a gente substituiu o nome do arquivo pela variável, precidido por um sinal de '$'. Esta é a forma de indicar uma variável no UNIX.

'-out $file.blast.txt' - o nome do arquivo de output do blast. Veja que nós utilizamos novamente a variável '$file', seguido por uma string que indica para a gente que esse é o output do blast.

Basicamente, o unix vai substituir '$file' pelo nome do arquivo em que o loop está.

No final desse loop, três arquivos serão produzidos. Um para cada transcriptoma. Mas este processo pode demorar um pouco.


## Interpretado os resultados do blast

Os resultados do blast são armazenados em um arquivo de text, com formato 'tsv'. Esse formato é basicamente uma tabela, em que as colunas estão separadas por um tab.

a opção '-outfmt 6' especifica quais informações são incluídas no arquivo que reporta dos resultados do blast.

**Atividades:** Utilize o google para encontrar quais as informações incluídas no '-outfmt 6'.

A seguir, nós queros saber quantas das sequências do transcriptoma podem ser anticoagulante seguindo o blast. Para isso bastaria contar qnts sequências há no output do blast. Porém, mesmo utilizando a opção '-max_target_seqs 1', que em teoria especifica para o blast reportar o melhor match, qnd a diferênça entre dois matches não é significativa, o blast reporta ambos os matches. 

Traduzindo, quando uma sequência é muito parecedida com duas sequências e a diferença entre a similaridade entre as sequências não é significativa, o blast vai gerar duas entradas para essa sequência. 

Desta forma, nós temos que eliminar essa redundância.

