# Computação Experimental 21/2
## Modelo Wolf Sheep Escape

O modelo foi modificado com base em um exemplo fornecido no repositório do  [framework MESA](https://github.com/projectmesa/mesa-examples).

O *Wolf_Sheep* consiste em um modelo ecológico com três agentes: lobos, ovelhas e grama. Os lobos e ovelhas transitam pelo grid randomicamente, e gastam energia para se movimentar. Para recuperá-la, devem comer. Ovelhas comem grama e os lobos comem ovelhas (desde que estejam no mesmo lugar).

Se lobos e ovelhas tiverem energia suficiente, podem reproduzir e gerar novos lobos e ovelhas (de forma asexuada, pois só é necessário um pai). Caso eles não tenham energia, morrem.

Com as modificações inseridas, há chances da ovelha conseguir escapar do lobo caso estejam no mesmo lugar do grid, e caso consiga, sua energia decai pela metade.

## Hipótese causal

Após a observação das execuções do modelo no framework mesa, foi formulada a seguinte hipótese:
"Caso as ovelhas tenham chance de escapar do lobos, poderá observar-se uma a diminuição no número de lobos vivos no ecossistema."


## Mudanças
Foram adicionadas as seguintes variáveis:
- `escapeRange`: Define a margem das chances da ovelha conseguir escapar do lobo, não há valor mínimo mas o valor máximo é 1.
- `escapeChance`: Cada ovelha possui uma chance aleatória (entre 0 e 1) de conseguir escapar.

O comportamento esperado foi do prevalecimento das ovelhas no ecossistema, uma vez que os lobos possuem menos chances de abatê-las. Assim, quanto maior o *range*, maiores são as chances disso ocorrer. 

O código referente ao abate das ovelhas foi modificado para comportar a lógica nova, além de diminuir a energia delas após o escape.

```
if  len(sheep) >  0:
	sheep_to_eat =  self.random.choice(sheep)
	if sheep_to_eat.escapeChance <=  self.model.escapeRange:
		self.energy +=  self.model.wolf_gain_from_food
		# Kill the sheep
		self.model.grid._remove_agent(self.pos, sheep_to_eat)
		self.model.schedule.remove(sheep_to_eat)

	else:
		sheep_to_eat.energy /=  2```
```
Além disso, foram adicionadas modificações para a exportação do *output* em .csv

## Instruções

### Dependências
O único pacote necessário é o Mesa, deve-se instalar a versão do github através do seguinte comando:
```
pip install -e git+https://github.com/projectmesa/mesa#egg=mesa

```

### Execução
O modelo em modo gráfico é aberto através do seguinte comando na raiz do diretório:
```
mesa runserver

```
O comando de geração dos arquivos .csv é:
```
python3 export.py
```

## Colunas

-   **RunId**: id da execução;
-   **iteration**: iteração atual;
-   **Step**: passo atual;
-   **width**: largura do grid;
-   **height**: altura do grid;
-   **sheep_reproduce**: probabilidade de reprodução das ovelhas;
-   **wolf_reproduce**: probabilidade de reprodução dos lobos;
-   **wolf_gain_from_food**: quanta energia os lobos ganham ao se alimentar;
-   **grass**: flag da grama;
-   **grass_regrowth_time**: taxa de recrescimento da grama;
-   **sheep_gain_from_food**: energia que as ovelhas ganham ao se alimentar;
-   **initial_sheep**: número de inicial de ovelhas;
-   **initial_wolves**: número inicial de lobos;
-   **escapeRange**: margem de probabilidade de escape;
-   **Wolves**: número de lobos vivos;
-   **Sheep**: número de ovelhas vivas;