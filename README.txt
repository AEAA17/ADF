Este código realiza uma análise financeira e operacional para uma empresa, concentrando-se nos gastos com folha salarial, faturamento, contratos fechados por funcionários e ticket médio mensal. A análise combina dados de funcionários, clientes e serviços prestados, gerando insights sobre a eficiência e o desempenho das áreas da empresa.

1. Importação de Módulos e Carregamento de Arquivos
As bibliotecas pandas e matplotlib foram importadas para manipulação e visualização de dados. Três conjuntos de dados foram carregados:

servicos: Dados de serviços prestados, incluindo informações sobre clientes e contratos.
clientes: Cadastro de clientes, contendo informações como o valor do contrato mensal.
funcionarios: Cadastro de funcionários, com dados sobre salários, benefícios e área de atuação.
Os dados foram carregados a partir de arquivos Excel e CSV e exibidos para verificação inicial.

2. Cálculo da Folha Salarial
A folha salarial total da empresa foi calculada somando as colunas de Salario Base, Impostos, Beneficios, VT (vale-transporte) e VR (vale-refeição) para todos os funcionários. O valor total foi impresso no formato monetário para fácil interpretação. Isso permite à empresa monitorar seus gastos com pessoal.

3. Cálculo do Faturamento da Empresa
O faturamento foi calculado combinando os dados de serviços prestados com os dados de clientes, associando o valor do contrato mensal de cada cliente ao tempo total de contrato em meses. O faturamento total foi obtido multiplicando o tempo de contrato pelo valor mensal e, em seguida, somando os valores para todos os contratos. Isso oferece uma visão clara da receita gerada pelos contratos em vigor.

4. Análise da Eficiência de Funcionários no Fechamento de Contratos
Foi calculado o percentual de funcionários que efetivamente fecharam contratos em comparação ao total de funcionários da empresa. Esse cálculo foi feito dividindo o número de funcionários que fecharam contratos pelo total de funcionários, resultando em um percentual que indica a eficiência da equipe de vendas ou operações.

5. Análise do Total de Contratos por Área
O código também analisou a distribuição dos contratos fechados por área de atuação dos funcionários. Os contratos foram agrupados pela área, e o total de contratos por área foi visualizado em um gráfico de barras. Isso permite identificar quais áreas da empresa são mais ativas em termos de fechamento de negócios.

6. Análise do Total de Funcionários por Área
Foi calculado o número de funcionários em cada área da empresa, e essa distribuição também foi visualizada em um gráfico de barras. Comparando este gráfico com o dos contratos por área, a empresa pode avaliar a produtividade relativa de cada departamento.

7. Cálculo do Ticket Médio Mensal
Por fim, o ticket médio mensal foi calculado, que representa o valor médio pago pelos clientes por mês. Esse valor é uma métrica importante para entender a receita média por cliente e pode ser utilizado para estratégias de precificação ou segmentação de clientes.


Código:

# Importando módulos e arquivos
import pandas as pd
import matplotlib.pyplot as plt

servicos = pd.read_excel(r"C:\Users\euric\Documents\Códigos Py\AD\Exercício Desafio pandas - Mini Projeto Análise de Dados\BaseServiçosPrestados.xlsx")
display(servicos)

clientes = pd.read_csv(r"C:\Users\euric\Documents\Códigos Py\AD\Exercício Desafio pandas - Mini Projeto Análise de Dados\CadastroClientes.csv", sep=';', decimal = ',' )
display(clientes)

funcionarios =pd.read_csv(r"C:\Users\euric\Documents\Códigos Py\AD\Exercício Desafio pandas - Mini Projeto Análise de Dados\CadastroFuncionarios.csv", sep=';', decimal = ',' )
display(funcionarios)

# Folha Salarial
folha = funcionarios['Salario Base'] + funcionarios['Impostos'] +funcionarios['Beneficios'] +funcionarios['VT'] +funcionarios['VR'] 
print("A folha salarial foi de: R${:,} ".format(folha.sum()))

# Faturamento da Empresa
fat = servicos[['ID Cliente' , 'Tempo Total de Contrato (Meses)']].merge(clientes[['ID Cliente','Valor Contrato Mensal']], on='ID Cliente')
fat_emp= fat['Tempo Total de Contrato (Meses)']*fat['Valor Contrato Mensal']
print("A folha salarial foi de: R${:,} ".format(fat_emp.sum()))

# Percentual de Funcionários que fechou contrato
qtdt= len(funcionarios['ID Funcionário'])
qtdf= len(servicos['ID Funcionário'].unique()) 
p= qtdf/qtdt
print("A quantidade de funcionários que fecharam contrato foi de: {:.2%} ".format(p))

# Total de Contratos por Área
ca = servicos[['ID Funcionário']].merge(funcionarios[['ID Funcionário','Area']], on='ID Funcionário')
tca = ca['Area'].value_counts()
print(tca)
tca.plot(kind='bar')
plt.show()

# Total de Funcionários por Área
tfa = funcionarios['Area'].value_counts()
print(tfa)
tfa.plot(kind='bar')
plt.show()

# Ticket Médio Mensal
tm = clientes['Valor Contrato Mensal'].mean()
print("O Ticket Médio Mensal foi de: R${:,.2f} ".format(tm))
