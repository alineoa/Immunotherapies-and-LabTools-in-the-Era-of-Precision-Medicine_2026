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
- [MolProbity](https://molprobity.lbl.gov/) — validar geometria e examinar o gráfico de Ramachandran.
- [QMEAN](https://swissmodel.expasy.org/qmean) - validar geometria via QMEANDisCo
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

### 3. Validar a geometria com MolProbity e QMEAN

1. Envie separadamente os modelos do ABodyBuilder2 e do AF3 ao [MolProbity](https://molprobity.lbl.gov/) e ao [QMEAN](https://swissmodel.expasy.org/qmean).
2. Execute a validação e examine o **gráfico de Ramachandran**.
3. Registre a porcentagem de resíduos em regiões favorecidas, permitidas e não permitidas (*outliers*). Verifique se os outliers estão em loops/CDRs ou no arcabouço. No QMEANDisCo, avalie as métricas de qualidade global e local.

A validação geométrica complementa as métricas de confiança: um modelo com confiança alta ainda pode apresentar problemas estereoquímicos.

### 4. Visualizar e comparar no ChimeraX

Use preferencialmente o **UCSF ChimeraX**. Abra os PDBs dos dois modelos e, quando necessário, a estrutura experimental [5B8C](https://www.rcsb.org/structure/5B8C).

**Para analisar uma estrutura**
- **Cartoon/Ribbon:** observar o dobramento, folhas β e hélices.
- **Surface:** avaliar a forma e a superfície acessível do anticorpo ou do PD-1.
- **Stick:** examinar resíduos específicos, contatos e cadeias laterais.
- **Colorir por cadeia:** distinguir VH e VL ou anticorpo e antígeno.
- **Colorir por pLDDT/confiança:** localizar regiões mais e menos confiáveis, se o campo estiver disponível no arquivo.

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
2. Carregue o modelo do Fv do pembrolizumab como **Receptor** e a estrutura de PD-1 como **Ligand**. Use arquivos PDB preparados e confira os IDs das cadeias. Se for utilizar o modelo AF3, é necessário converter o arquivo mmCIF para PDB.
3. Em *Advanced Options*, ative **Antibody Mode**.
4. Não imponha os resíduos do BepiPred como restrições nesta etapa. O objetivo é avaliar onde as poses geradas se posicionam; a comparação com o epítopo previsto será feita depois.
5. Baixe algumas poses bem ranqueadas e abra-as no ChimeraX. Verifique se as CDRs participam da interface, se as poses convergem para uma região semelhante e se a interface coincide com a região observada em 5B8C e/ou prevista pelo BepiPred.

**Produto:** seleção de poses representativas e uma comparação breve entre a interface prevista e a experimental.

### 7. Discussão

- As CDRs estão bem modeladas nos dois modelos? Quais evidências sustentam essa avaliação?
- O que mostram pLDDT/RMSPE e MolProbity? As métricas apontam para as mesmas regiões problemáticas?
- O alinhamento com **Matchmaker** revela diferenças importantes entre os modelos e a estrutura experimental?
- As poses do ClusPro reproduzem a região de ligação conhecida? Quais são as limitações dessa comparação?

## Estudo de caso 2 — Design *de novo* de anticorpos contra PD-1

Use a estrutura experimental de PD-1 disponível no [PDB 5B8C](https://www.rcsb.org/structure/5B8C) ou outra estrutura adequada, removendo as cadeias desnecessárias. Escolha a superfície-alvo e confira a numeração dos resíduos diretamente no arquivo carregado.

1. Acesse [RFantibody](https://app.tamarind.bio/tools/rfantibody), se disponível para a atividade.
2. Carregue a estrutura de PD-1 e selecione a cadeia correta do antígeno.
3. Escolha resíduos *hotspot* na superfície de interesse. Confirme os números no PDB; não copie a numeração de outra estrutura sem verificar.
4. Gere um pequeno conjunto de candidatos e salve as sequências e estruturas retornadas.
5. Abra os complexos no ChimeraX. Examine a orientação das CDRs, colisões evidentes, tamanho da interface e contatos com os *hotspots*.
6. Acesse o [TAP2](https://app.tamarind.bio/tools/tap) e submeta as sequências das regiões variáveis das cadeias pesada (VH) e leve (VL) de cada candidato. Registre as métricas disponíveis, incluindo comprimento total das CDRs, patches de hidrofobicidade superficial, patches de carga positiva e negativa e simetria de carga do Fv. Identifique possíveis características desfavoráveis à solubilidade, expressão ou estabilidade e compare os resultados entre os candidatos.
7. Acesse o [BioPhi](https://app.tamarind.bio/tools/biophi) e analise as sequências VH e VL. Registre o escore de *humanness* OASis, quando disponível, e examine possíveis sugestões de humanização. Compare as sequências originais e modificadas, identificando alterações em regiões estruturais e CDRs. Discuta possíveis benefícios e riscos dessas modificações, considerando que maior similaridade com sequências humanas não garante menor imunogenicidade nem preservação da ligação ao antígeno.
8. Acesse o [Aggrescan3D](https://app.tamarind.bio/tools/aggrescan3d) e submeta as estruturas dos candidatos. Compare os escores de propensão à agregação e identifique regiões com valores elevados. Visualize essas regiões no ChimeraX e verifique se estão localizadas nas CDRs, no arcabouço do anticorpo ou em superfícies expostas. Sempre que possível, utilize estruturas e configurações equivalentes para comparar os candidatos.
9. Selecione alguns candidatos para comparação integrada. Relacione os resultados do TAP2, BioPhi e Aggrescan3D com as características estruturais observadas no ChimeraX. Discuta quais candidatos parecem mais promissores considerando a geometria do complexo, as propriedades de *developability* e a similaridade com sequências humanas. Quando comparar estruturas homólogas, utilize **Matchmaker** e confirme que as cadeias correspondem corretamente.

**Questões para discussão**

- Os candidatos apresentam orientações das CDRs compatíveis com o reconhecimento da superfície escolhida em PD-1?
- Os candidatos com melhores características de interação também apresentam perfis favoráveis de *developability*?
- Os resultados do TAP2 e do Aggrescan3D são consistentes? Que informações complementares cada ferramenta fornece?
- As sugestões do BioPhi modificam regiões importantes para o reconhecimento do antígeno? Como essas alterações poderiam ser avaliadas?
- Quais candidatos você priorizaria para validação experimental e quais evidências computacionais sustentam essa escolha?

Os candidatos gerados e as métricas obtidas são hipóteses computacionais, não evidências experimentais. A aparência da pose ou os escores de predição, isoladamente, não demonstram ligação, afinidade, especificidade, estabilidade, solubilidade ou atividade biológica. A confirmação dessas propriedades exige validação experimental.
