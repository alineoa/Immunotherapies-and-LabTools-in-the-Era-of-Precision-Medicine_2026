# Bioinformática estrutural aplicada a anticorpos
## Da sequência à análise da interface e ao design de anticorpos contra PD-1

### Sistema-modelo

O alvo desta aula é o receptor humano PD-1 (Programmed cell death protein 1, gene *PDCD1*), um receptor imunorregulador envolvido no controle da resposta imune. O pembrolizumab é um anticorpo terapêutico que se liga ao PD-1 e será usado como exemplo para modelagem e análise estrutural.

Estrutura experimental de referência: [PDB 5B8C — PD-1 em complexo com o Fv do pembrolizumab](https://www.rcsb.org/structure/5B8C). Ela será usada para comparar os modelos computacionais com uma estrutura experimental.

### Ferramentas

- [DrugBank](https://go.drugbank.com/) — localizar a entrada do pembrolizumab e consultar suas sequências.
- [ANARCI](https://opig.stats.ox.ac.uk/webapps/anarci/) — numerar e identificar as regiões variáveis e CDRs.
- [ABodyBuilder2](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/abodybuilder2/) — modelar o Fv a partir de VH e VL.
- [AlphaFold Server](https://alphafoldserver.com/) — predizer a estrutura do anticorpo ou do complexo, conforme os tipos de entrada aceitos pelo servidor.
- [BepiPred-3.0](https://services.healthtech.dtu.dk/services/BepiPred-3.0/) — prever regiões candidatas a epítopos a partir da sequência do PD-1.
- [ClusPro](https://cluspro.org/) — docking proteína–proteína, usando **Antibody Mode**.
- [MolProbity](https://molprobity.biochem.duke.edu/) — validar geometria e examinar o gráfico de Ramachandran.
- [UCSF ChimeraX](https://www.cgl.ucsf.edu/chimerax/) — visualização e comparação estrutural.

## Estudo de caso 1 — Modelagem e análise do pembrolizumab–PD-1

### 1. Obter e identificar as sequências do pembrolizumab

1. Acesse o [DrugBank](https://go.drugbank.com/) e procure por **pembrolizumab**. Abra a entrada correspondente e localize as sequências das cadeias pesada e leve.
2. Copie as sequências para arquivos FASTA separados. Para modelar o Fv, use as regiões variáveis (VH e VL), sem as regiões constantes.
3. Submeta as sequências ao **ANARCI**. Identifique a numeração utilizada, as regiões variáveis e as CDRs de cada cadeia. Salve o resultado para consulta durante a análise.

**Produto:** sequências VH/VL identificadas e numeradas, com as CDRs anotadas.

### 2. Modelar o anticorpo com duas ferramentas

**ABodyBuilder2**
1. Abra o servidor e cole VH em *Heavy chain sequence* e VL em *Light chain sequence*.
2. Execute a modelagem e baixe o PDB do Fv.
3. Abra o arquivo PDB em um editor de texto e procure o campo de confiança/erro por resíduo, quando presente. Registre o **RMSPE** informado pelo ABodyBuilder2 e observe se há regiões com valores mais altos.

**AlphaFold Server (AF3)**
1. Acesse o [AlphaFold Server](https://alphafoldserver.com/) e siga as instruções para submeter as sequências do anticorpo, respeitando os tipos de entrada aceitos pela interface.
2. Baixe o modelo e abra o arquivo PDB/mmCIF em um editor de texto.
3. Localize o campo **pLDDT** (geralmente armazenado como fator B/`B-factor` nos arquivos PDB exportados) e registre os valores. Se houver valores por resíduo, observe especialmente as CDRs.

**Importante:** pLDDT e RMSPE são métricas diferentes e não devem ser comparadas numericamente entre si. Registre qual métrica pertence a cada ferramenta e interprete a confiança por região, não apenas pelo valor global. A disponibilidade e a forma de exportação dessas métricas podem variar conforme o servidor e o formato do arquivo.

### 3. Validar a geometria com MolProbity

1. Envie separadamente os modelos do ABodyBuilder2 e do AF3 ao [MolProbity](https://molprobity.biochem.duke.edu/).
2. Execute a validação e examine o **gráfico de Ramachandran**.
3. Registre a porcentagem de resíduos em regiões favorecidas, permitidas e não permitidas (*outliers*). Verifique se os outliers estão em loops/CDRs ou no arcabouço.

A validação geométrica complementa as métricas de confiança: um modelo com confiança alta ainda pode apresentar problemas estereoquímicos.

### 4. Visualizar e comparar no ChimeraX

Use preferencialmente o **UCSF ChimeraX**. Abra os PDBs dos dois modelos e, quando necessário, a estrutura experimental [5B8C](https://www.rcsb.org/structure/5B8C).

**Para analisar uma estrutura**
- **Cartoon/Ribbon:** observar o dobramento, folhas β e hélices.
- **Surface:** avaliar a forma e a superfície acessível do anticorpo ou do PD-1.
- **Stick:** examinar resíduos específicos, contatos e cadeias laterais.
- **Colorir por cadeia:** distinguir VH e VL ou anticorpo e antígeno.
- **Colorir por pLDDT/confiança:** localizar regiões mais e menos confiáveis, se o campo estiver disponível no arquivo.
- **Mostrar CDRs:** usar a numeração do ANARCI para localizar CDR-H1/H2/H3 e CDR-L1/L2/L3.

**Para comparar estruturas**
Use **Matchmaker** no ChimeraX para alinhar duas estruturas homólogas, por exemplo, o modelo do Fv e a região Fv do pembrolizumab na estrutura experimental. Compare o alinhamento global e as diferenças nas CDRs. Confira se as cadeias correspondem antes de executar o alinhamento; não compare diretamente cadeias de identidade diferente.

### 5. Predizer regiões candidatas a epítopo no PD-1

1. Obtenha a sequência do domínio extracelular humano de PD-1 a partir de uma fonte de sequência confiável, como [UniProt](https://www.uniprot.org/), e use-a no [BepiPred-3.0](https://services.healthtech.dtu.dk/services/BepiPred-3.0/).
2. Examine o gráfico e anote os resíduos acima do limiar selecionado.
3. Mapeie os resíduos candidatos na estrutura do PD-1 no ChimeraX. Verifique se estão expostos ao solvente e se formam uma região espacialmente coerente.
4. Compare-os com a interface experimental observada em 5B8C.

**Atenção:** BepiPred identifica regiões candidatas; não confirma qual epítopo é reconhecido por um anticorpo específico. Epítopos conformacionais podem reunir resíduos distantes na sequência, mas próximos no espaço.

### 6. Docking com ClusPro — Antibody Mode

1. Acesse o [ClusPro](https://cluspro.org/) e inicie um trabalho.
2. Carregue o modelo do Fv do pembrolizumab como **Receptor** e a estrutura de PD-1 como **Ligand**. Use arquivos PDB preparados e confira os IDs das cadeias.
3. Em *Advanced Options*, ative **Antibody Mode** e, se disponível, **Automatically Mask non-CDR regions**.
4. Não imponha os resíduos do BepiPred como restrições nesta etapa. O objetivo é avaliar onde as poses geradas se posicionam; a comparação com o epítopo previsto será feita depois.
5. Baixe algumas poses bem ranqueadas e abra-as no ChimeraX. Verifique se as CDRs participam da interface, se as poses convergem para uma região semelhante e se a interface coincide com a região observada em 5B8C e/ou prevista pelo BepiPred.

**Produto:** seleção de poses representativas e uma comparação breve entre a interface prevista e a experimental.

### 7. Discussão

- As CDRs estão bem modeladas nos dois modelos? Quais evidências sustentam essa avaliação?
- O que mostram pLDDT/RMSPE e MolProbity? As métricas apontam para as mesmas regiões problemáticas?
- O alinhamento com **Matchmaker** revela diferenças importantes entre os modelos e a estrutura experimental?
- As poses do ClusPro reproduzem a região de ligação conhecida? Quais são as limitações dessa comparação?

## Estudo de caso 2 — Design *de novo* de anticorpos contra PD-1

Use a estrutura experimental de PD-1 em [5B8C](https://www.rcsb.org/structure/5B8C) ou outra estrutura de PD-1 adequada, removendo as cadeias que não forem necessárias. Escolha a superfície-alvo e confira a numeração dos resíduos diretamente no arquivo carregado.

1. Acesse [RFantibody](https://app.tamarind.bio/tools/rfantibody), se disponível para a atividade.
2. Carregue a estrutura de PD-1 e selecione a cadeia correta do antígeno.
3. Escolha resíduos *hotspot* na superfície de interesse. Confirme os números no PDB; não copie a numeração de outra estrutura sem verificar.
4. Gere um pequeno conjunto de candidatos e salve as sequências e estruturas retornadas.
5. Abra os complexos no ChimeraX. Examine a orientação das CDRs, colisões evidentes, tamanho da interface e contatos com os hotspots.
6. Selecione alguns candidatos para comparação. Quando comparar modelos, use **Matchmaker** apenas para estruturas homólogas e confirme que as cadeias correspondem.

Os candidatos são hipóteses computacionais, não anticorpos validados. A aparência da pose, isoladamente, não demonstra ligação, afinidade, especificidade ou atividade biológica.
