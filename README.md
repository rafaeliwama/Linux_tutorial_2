# Tutorial Linux 2
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

Desta forma, nós temos que eliminar essa redundância, mas nós queremos manter o melhor hit. Para isso, nós precisamos do hit com menor e-value que é a coluna 11

Nós usaremos 4 comandos em série para: 'sort', 'awk', 'uniq' e 'wc'.


```
sort -k1 -k12 GBRF01.1.fsa_nt.blast.txt | awk -F '\t' '{print $1}' | uniq | wc -l
```

Note que nós adicionamos um elemento que nunca foi utilizado antes.

O caractere '|' chama-se pipe. O pipe é abreviação para pipeline e ele possibilita a utilização de varios comandos em série. Basicamente ele redireciona o output do comando anterior para o próximo.

O comando 'sort' reorganiza o arquivo em ordem crescente (default), as opções '-k1' e '-k2' indicam que as colunas 1 e 2 serão utilizadas para fazer o sort, mas o sort ocorrerá primeiro na coluna 1.

O segundo comando é o 'awk' e esse programa é um inferno para entender. Ele é mais ou menos iqual o grep, que filtra o seu arquivo de várias maneiras. O 'awk' pode ser utilizado para isolar colunas em um arquivo 'tsv' ou qualquer arquivo que contenha colunas. A opção '-F' especifica qual caractere separa as colunas. No nosso caso é o tab, que é indicado pela expressão regular '/t'. E por fim, essa é a parte dificil de entender. O que está entre colchetes é o que você quer que o 'awk' faça. Nesse caso, printar a coluna $1. Para saber mais sobre o 'awk' veja um guia aqui: https://www.certificacaolinux.com.br/comando-awk-no-linux-processa-dados-guia-basico/

O 'uniq' é um comando muito interessante. Ele colapsa inputs que são repetidos. Porém, ele colapsa somente inputs que sejam adjacentes um ao outro. Por exemplo:

Na lista seguinte, a entrada 'aa' é seguida diretamente por outra entrada 'aa', mas a entrada 'bb' é seguida por um 'cc'.

**Lista:**

aa

aa

bb

cc

bb


O 'uniq' colapsa 'aa', mas não colapsa bb. É exatamente por isso que nós precisamos fazer o 'sort' antes do 'uniq', para que as entradas de uma mesma sequencia estejam todas uma após a outra!

**Atividade:** cheque a quantidade de sequências que podem ser anticoagulantes em cada um dos transcriptomas utilizando o exemplo da linha de comando acima.

## Concatenando arquivos

Para facilitar a nossa vida, é muito comum nós concatenarmos, juntar, vários arquivos em um único. Isso evita que nós tenhamos que manter vários arquivos. Vira uma bagunça quando a gente tem muitos arquivos. Esse arquivo concatenado, é normalmente chamado de 'master file', porque contém a toda a informação de um determinado projeto.

concatenar arquivos é super fácil. nós utilizamos a seguinte sintaxe:

```
file_1 >> master_file
file_2 >> master_file
file_3 >> master_file
.
.
.
```

Observe que '>>' é diferente de '>'. Os dois redirecionam o output para um arquivo. Contudo, o comando '>' apaga o conteúdo do arquivo que o output é redirecionado, enquanto '>>' emenda o arquivo com o output.

Por exemplo:

Como estamos em clima de recepção, já vai preparando o arquivo pra mandar pros os bixos:
![madagascar](https://user-images.githubusercontent.com/46658489/225446995-ea2cca17-925c-481a-bf9a-9197d7694b6d.jpg)


```
echo Ela é tão tudo! Ela é tão tudo! >> madagascar_refrão.txt
echo Tudo que eu queria abraçar, beijar >> madagascar_refrão.txt
echo Ela é tão tudo! Ela é tão tudo! >> madagascar_refrão.txt
echo Vou partir com ela pra madagascar >> madagascar_refrão.txt
```
Agora, faça a mesma sequência de comandos utilizando o '>' e redirecione para um arquivo madagascar_refrão_2.txt

Inspecione o conteúdo dos dois arquivos.


Ta bom. Então sabemos que podemos usar '>>'. Mas lembre que qnd a gente juntar todos os arquivos, nós não vamos saber de qual arquivo cada entrada vem. Além disso, pode haver mais de uma sequência com o mesmo identificador, já que estes transcriptomas são montados de forma independente.

Para isso, é uma boa ideia alterar minimamente o identificador da sequência, pq que a gente saiba facilmente de qual espécie vem esta sequência.

Nós vamos alterar os identificadores das sequências com o comando 'sed'. Este comando é um dos comandos mais poderosos do unix. Este manual é um dos melhores para o 'sed': https://www.computerhope.com/unix/used.htm

O 'sed' tem a seguinte sintaxe:

```
sed OPTIONS... [SCRIPT] [INPUTFILE...]
```

O script é dividido em três elementos: command, regular_expressions_find, regular_expressions_replace, tag

Os comandos do 'sed' são infinitos. Cada um faz uma coisa diferente e aquele site tem a descrição de todos. Nós vamos utilizar o 'S', que é o de substituição e a tag 'G' que faz o match com TODAS as ocorrências no input.

Relação de espécies, respectivos identificadores e prefixos:

Hirudo medicinalis - GBRF01 - HirMed
Americobdella valdiviana - GIVY01 - AmeVal
Patagoniobdella sp. - GIWB01 - PatSp

Nós vamos adicionar os prefixos nos identificadores de cada entrada na output do blast.


```
sed -i 's/GBRF/HirMed_GBRF/g' GBRF01.1.fsa_nt.blast.txt
```

A opção '-i' escreve o output do sed no próprio arquivo.

**Atividade:** faça as substituições nos outros arquivos com a tabela acima e junto todos os arquivos em um master_blast_table.tsv.

Blz! Agora, quantas dessas sequências possuem hits contra anticoagulantes que têm a função testada?

Nós precisamos procurar por sequências com o hits contra as sequências naquela lista que nós geramos no tutorial anterior. É facil. É só usar o grep.

```
grep -f anticogs_tested.txt master_blast_table.tsv > true_anticogs.tsv
```

A opção '-f' faz com que os padrões de input do grep sejam lidos de um arquivo (aquela lista), e a gente redireciona os resultados para o true_anticogs.tsv. Inspecione o arquivo true_anticogs.tsv e conte quantas sequencias estão contidas nele.

## O que vc aprendeu:

1. O básico sobre instalar softwares utilizando as linhas de comando;

2. Utilizar o Blast;

3. Utilização de loops;

4. Comandos 'sort', 'awk' e 'uniq';

5. O comando 'sed'

6. Concatenamento de arquivos com '>>'

7. Como criar um arquivo com a letra de madagascar.


